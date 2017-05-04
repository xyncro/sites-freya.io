Getting Started
===============

You can be up and running with Freya in minutes! This quick guide will take you from zero to Hello World, using some of the high level building blocks that Freya provides out of the box -- and tell you where to go to learn more about each of them.

Your Hello World will be completely self contained, and able to greet individuals by name if they provide one -- reverting to a classic Hello World if not. It's certainly possible to write this in a simpler way using Freya, but this implementation introduces some of the key Freya building blocks which will be valuable as you build more complex applications. Here's the complete code, before the implementation is broken down:

.. code-block:: fsharp

   open Freya.Core
   open Freya.Machines.Http
   open Freya.Routers.Uri.Template

   let name =
       freya {
           let! name = Freya.Optic.get (Route.atom_ "name")

           match name with
           | Some name -> return name
           | _ -> return "World" }

   let hello =
       freya {
           let! name = name

           return Represent.text (sprintf "Hello %s!" name) }

   let machine =
       freyaMachine {
           handleOk hello }

   let router =
       freyaRouter {
           resource "/hello{/name}" machine }

   type HelloWorld () =
       member __.Configuration () =
           OwinAppFunc.ofFreya (router)

   open System
   open Microsoft.Owin.Hosting

   [<EntryPoint>]
   let main _ =

       let _ = WebApp.Start<HelloWorld> ("http://localhost:7000")
       let _ = Console.ReadLine ()

       0

Dependencies
------------

You'll be creating the Hello World implementation as a simple F# console application, so go ahead and create a new empty F# console application. As you can see above, you can fit the whole thing in the single file that makes up the program (easily!) so there's no need to worry about complex application structures or any of the complexities of frameworks like ASP.NET MVC.

.. hint::

   In general, `Paket <https://fsprojects.github.io/Paket>`_ comes highly recommended as a reliable way of managing your Nuget -- and other -- dependencies. For this guide however, standard Nuget tools will work well enough.

The first thing you'll need is Freya and a suitable web server. In this case, the simple server from the Katana project will be used, although other servers like Kestrel, IIS, etc. will also work.

Using your preferred method of managing Nuget dependencies, install the required packages. Make sure you're using the latest available versions!

.. code-block:: shell

   PM> Install-Package Freya -Pre
   PM> Install-Package Microsoft.Owin.SelfHost

The Freya package is a meta-package -- it brings in all of the packages you'll need for a common Freya application.

Code
----

At this point you should have an empty F# program. Start off by opening some common namespaces we'll need to write our Hello World program. In this case, you'll need three parts of the Freya stack.

.. code-block:: fsharp

   open Freya.Core
   open Freya.Machines.Http
   open Freya.Routers.Uri.Template

Greeting
^^^^^^^^
   
Now you're ready to implement the Freya part of your Hello World application. Start by creating these two functions:

.. code-block:: fsharp

   let name =
       freya {
           let! name = Freya.Optic.get (Route.atom_ "name")

           match name with
           | Some name -> return name
           | _ -> return "World" }

   let hello =
       freya {
           let! name = name

           return Represent.text (sprintf "Hello %s!" name) }

You now have two functions which when used together return a representation of ``Hello [World|{name}]`` depending on whether ``{name}`` was present in the route. You'll note that these functions are computation expressions -- these are very common in Freya and form the basis of the programming model (although computation expression syntax is optional). For more on functions in Freya, see the :doc:`/reference/core/index` reference. You'll see more about routing in a following section.

Resource
^^^^^^^^

Now you need some way of handling a request and using your ``hello`` function to return the representation of your greeting as the response -- you need a way to model an HTTP resource. You can use the Freya HTTP Machine to do this. Machines are a powerful and high level abstraction -- see the :doc:`/reference/machines/index` reference for more, but for now you can simply use the very simply configured machine below, which will return your representation when a normal "OK" response is valid.

.. code-block:: fsharp

   let machine =
       freyaMachine {
           handleOk hello }

Router
^^^^^^

Finally, you'll need a way to make sure that requests to the appropriate path(s) end up at your new Machine-based resource. You can use the URI Template based Freya router to do this easily. The following function will give you a simple router which will route requests matching the given path to your machine. For more on routing in Freya, see the :doc:`/reference/routers/index` reference.

.. code-block:: fsharp

   let router =
       freyaRouter {
           resource "/hello{/name}" machine }

Server
------

Now that you have all the "logic" covered you'll need a way of serving it. You can use a simple self-hosted server, and fire it up in the main method of your program. As you're using Katana here, you'll need to create a type of a suitable shape for Katana to use as a start-up object. Here's the code you'll need, along with a main method to start things up.

.. code-block:: fsharp

   type HelloWorld () =
       member __.Configure () =
           OwinAppFunc.ofFreya (router)

   open System
   open Microsoft.Owin.Hosting

   [<EntryPoint>]
   let main _ =

       let _ = WebApp.Start<HelloWorld> ("http://localhost:7000")
       let _ = Console.ReadLine ()

       0

And there you have it! Try hitting `localhost:7000/hello <http://localhost:7000/hello>`_ or `localhost:7000/hello/name <http://localhost:7000/hello/name>`_ in a browser -- you should have a Hello World up and running.

.. hint::

   The code for the simple Freya Hello World example can be found in the `freya-examples <https://github.com/xyncro/freya-examples>`_ GitHub repository `here <https://github.com/xyncro/freya-examples/blob/master/src/HelloWorld/Program.fs>`_ - if you have any problems, try cloning and running the pre-built example.

Hopefully now you're keen to learn more about the Freya components you've seen and what more they can do -- and what others are available. The rest of the Freya documentation should help -- and if you find it doesn't, please reach out and suggest improvements -- :doc:`/meta/contact` is a good place to begin.
