Getting Started on .NET Core
============================

.. note::

    Still on the .NET Framework? Check out our alternative guide to :doc:`/tutorials/getting-started` on the .NET Framework.

You can be up and running with Freya in minutes! This quick guide will take you
from zero to Hello World, using some of the high level building blocks that
Freya provides out of the box -- and tell you where to go to learn more about
each of them.

Your Hello World will be completely self contained, and able to greet
individuals by name if they provide one -- reverting to a classic Hello World if
not. It's certainly possible to write this in a simpler way using Freya, but
this implementation introduces some of the key Freya building blocks which will
be valuable as you build more complex applications.

Before we get started, you will want to install the Freya project templates for
.NET Core. You can do this using the :program:`dotnet` CLI tool.

.. code-block:: shell

    dotnet new --install Freya.Template::*

This process will take a few minutes to install the templates on your host,
after which, you can create a Freya project by simply typing

.. code-block:: shell

    dotnet new freya

.. hint::

    By default, the Freya template will use the ``Async`` construct from ``FSharp.Core``. If you want to use the ``Job`` construct from `Hopac <https://hopac.github.io/Hopac/Hopac.html>`_, then create the project using ``dotnet new freya -f hopac``.

This will create a basic Freya project for you, including four files. :file:`Api.fs`,
:file:`KestrelInterop.fs`, :file:`Program.fs`, and a :file:`{Freya}.fsproj` file.

First, let's take a look at :file:`Api.fs`. Here's the complete code,
before the implementation is broken down:

.. code-block:: fsharp

    module Api

    open Freya.Core
    open Freya.Machines.Http
    open Freya.Types.Http
    open Freya.Routers.Uri.Template

    let name_ = Route.atom_ "name"

    let name =
        freya {
            let! name = Freya.Optic.get name_

            match name with
            | Some name -> return name
            | None -> return "World" }

    let sayHello =
        freya {
            let! name = name

            return Represent.text (sprintf "Hello, %s!" name) }

    let helloMachine =
        freyaMachine {
            methods [GET; HEAD; OPTIONS]
            handleOk sayHello }

    let root =
        freyaRouter {
            resource "/hello{/name}" helloMachine }

Dependencies
------------

On .NET Core, the primary server supported by Freya is Kestrel, but any OWIN-compatible server can be used. The default template already pulls in the necessary dependencies on Kestrel and its OWIN bindings, so you don't need to change anything there.

Freya is distributed as a meta-package. The project file only includes a reference to the ``Freya`` package, which brings in all of the dependencies needed for a common Freya application.

Code
----

At this point you should have a basic Freya application. :file:`Api.fs` starts off by opening some common namespaces that are needed to access the major components of the Freya stack.

.. code-block:: fsharp

   open Freya.Core
   open Freya.Machines.Http
   open Freya.Types.Http
   open Freya.Routers.Uri.Template

Greeting
^^^^^^^^

The next section deals with the core ``freya`` construct, and is broken into three parts:

.. code-block:: fsharp

    let name_ = Route.atom_ "name"

    let name =
        freya {
            let! name = Freya.Optic.get name_

            match name with
            | Some name -> return name
            | None -> return "World" }

    let sayHello =
        freya {
            let! name = name

            return Represent.text (sprintf "Hello, %s!" name) }

The first line defines a constant prism that inspects the route of the request for a component named ``{name}`` and provides a view that makes it easy to access.

In the second definition, we use that prism to extract the name from the route. We then return either ``{name}`` or ``World`` depending on whether ``{name}`` was present in the route.

The third definition binds it all together, extracting the name and returning a representation of ``Hello [World|{name}]``. A representation is an important construct that binds together the data with its description, in this case :mimetype:`text/plain` in UTF-8 encoding.

You'll note that the second and third definitions are computation expressions -- these are very common in Freya and form the basis of the programming model (although computation expression syntax is optional). For more on functions in Freya, see the :doc:`/reference/core/index` reference. You'll see more about routing in a following section.

Resource
^^^^^^^^

Now we need some way of handling a request and using ``sayHello`` to return the representation of a greeting as the response -- we need a way to model an HTTP resource. Freya provides the Freya HTTP Machine to do this for us. Machines are a powerful and high level abstraction -- see the :doc:`/reference/machines/index` reference for more, but for now we simply use the machine as configured below, which responds to a ``GET``, ``HEAD``, or ``OPTIONS`` request with the "Hello, World!" greeting.

.. code-block:: fsharp

    let helloMachine =
        freyaMachine {
            methods [GET; HEAD; OPTIONS]
            handleOk sayHello }

Router
^^^^^^

Finally, we need a way to make sure that requests to the appropriate path(s) end up at the respective Machine-based resource. Freya uses the URI Template based Freya router to make this part easy. The template provides us with a simple router which routes requests matching the ``/hello{/name}`` path to ``helloMachine``. For more on routing in Freya, see the :doc:`/reference/routers/index` reference.

.. code-block:: fsharp

    let root =
        freyaRouter {
            resource "/hello{/name}" helloMachine }

Server
------

Now that all the "logic" has been covered we need a way of serving it. The template project provides :file:``KestrelInterop.fs`` which makes interoperating with Kestrel from F# a bit easier. Everything comes together in :file:``Program.fs``, which takes care of starting up the web server and configuring it to use our router as the API root.

.. code-block:: fsharp

    module Program

    open KestrelInterop

    [<EntryPoint>]
    let main _ =
      let configureApp =
        ApplicationBuilder.useFreya Api.root

      WebHost.create ()
      |> WebHost.bindTo [|"http://localhost:5000"|]
      |> WebHost.configure configureApp
      |> WebHost.buildAndRun

      0

Execution
---------

Now that we've gone over the pieces of our initial Freya application, we need to get it running. To do this, we again invoke the :program:`dotnet` CLI.

.. code-block:: shell

    dotnet restore
    dotnet run

And there you have it! After a few moments, the server will be ready to respond to requests. Try hitting `localhost:5000/hello <http://localhost:5000/hello>`_ or `localhost:5000/hello/name <http://localhost:5000/hello/name>`_ in a browser -- you should have a Hello World up and running.

Hopefully now you're keen to learn more about the Freya components you've seen and what more they can do -- and what others are available. The rest of the Freya documentation should help -- and if you find it doesn't, please reach out and suggest improvements -- :doc:`/meta/contact` is a good place to begin.
