Overview
========

To make the most of the HTTP machine, you should have a basic view of the model of a machine, ideally having read the topic on :doc:`/topics/design/machines` design. That section finished by hinting at the application of the machine model to the Freya HTTP machine -- this is covered in this and the following sections.

Syntax
------

The first thing to note is that as with the Freya URI Template Router, the configuration of the HTTP machine is done using a moderately declarative computation expression syntax. This allows for configuration to be readable while also taking advantage of some features of the F# type system to allow for concision where possible.

To create a simple HTTP machine, which models and HTTP resource, you can use the computation expression, ``freyaHttpMachine`` -- also aliased as ``freyaMachine`` for convenience:

.. code-block:: fsharp

   open Freya.Machine.Http

   let resource =
       freyaMachine {
          handleOk handler }

Here you can see a very simple resource with almost minimal configuration.

The computation expression makes extensive use of custom operations, giving a declarative syntax which allows for a very clean description of the functionality of the machine as defined by configuration.

Terminology
-----------

In the topic on :doc:`/topics/design/machines`, the application of the general machine model to specific problems, in this case HTTP was discussed, stating that general machine terminology is sometimes overridden for specific problem domains. The HTTP machine is one such example.

Decisions
^^^^^^^^^

Decisions remain the same, as they are universally applicable, and you will see decisions referred to throughout the HTTP machine documentation.

Terminals/Handlers
^^^^^^^^^^^^^^^^^^

As you can infer from the section title, the more general term Terminal is replaced by the HTTP-implying term Handler. This ties the approach back towards styles of web development which people are likely to be more familiar with, and anchors the concept in common terminology. In essence, you can think of an HTTP machine as making a series of Decisions to decide which Handler should handle an HTTP request, and return a suitable response.

Next
----

To understand how the HTTP machine works, it is useful to be able to view the design of the decision tree which has been modelled, and which the configuration provided using the computation expression will configure. The next section on the HTTP :doc:`model` gives information on the model, followed by a further section on individual :doc:`configuration` specifications.
