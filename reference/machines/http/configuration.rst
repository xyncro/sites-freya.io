Configuration
=============

There is extensive configuration available as part of the Freya HTTP machine, and the following sections deal with the different aspects, explained in the overview.

* :doc:`configuration/decisions` -- decisions influence the overall execution of the HTTP Machine, and enable you to define the behaviour of key aspects of your resource by answering true/false questions about the resource.
* :doc:`configuration/handlers` -- handlers enable the response to return representations of the resource when appropriate, as well as providing a potential point in the execution of the Machine to set additional headers, etc.
* :doc:`configuration/properties` -- properties of the HTTP resource which may be used by decisions to configure behaviour.
  
.. toctree::
   :hidden:

   configuration/decisions
   configuration/handlers
   configuration/properties
