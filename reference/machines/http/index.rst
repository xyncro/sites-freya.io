HTTP
====

Freya lets you work with HTTP implicitly, using the typed but low-level tools found in :doc:`/reference/core/index` combined with types and optics, such as the provided :doc:`/reference/optics/index`. These are expressive and effective, but do not impose a particular structure or semantic form.

.. hint::

   If you are already familiar with the HTTP machine, you may be looking to jump straight to the `Reference Diagrams <../../../_static/components.core.html>`_, or to the configuration reference for :doc:`configuration/properties`, :doc:`configuration/decisions` or :doc:`configuration/handlers`.

HTTP is a fairly involved set of standards, and properly implementing the correct semantics of HTTP is not a trivial matter. Dealing with the correct logical approach to negotiating content types, working out the right approach to cache control and correctly expressing the state of the underlying resources, can be quite a lot to design when taken holistically.

The Freya HTTP macine (**Freya.Machines.Http**) is designed to solve this. The Machine lets you define just the properties of a resource you care about, and the Machine library handles the rest. You can specify as much or as little as you wish, all in a type safe manner, and be confident that HTTP semantics will remain consistent and correct.

To begin working with the Freya HTTP machine, it is suggested that reading the topic around the design of :doc:`/topics/design/machines` makes a good entry point. Once you have an initial sense of the machine model, the following sections will should be easier to dive in to.

* :doc:`overview` -- the basic design and syntax of the Freya HTTP machine.
* :doc:`model` -- the diagram and implementation of the HTTP machine.
* :doc:`configuration` -- the available configuration options as part of the Freya HTTP machine, as introduced in the machine overview and model.

.. toctree::
   :hidden:

   overview
   model
   configuration
