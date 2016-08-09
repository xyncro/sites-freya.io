Home
====

`Freya <https://freya.io>`_ is a functional web programming stack for F#. Freya emphasises expressivity, safety, and correctness and is compatible with most server technologies in the .NET space.

Getting Started
---------------

The quickest way to get up and running with Freya and to get a feeling for what programming with Freya is like, is to dive straight in to the :doc:`/topics/getting-started/index` topic. From zero to a Freya-based Hello World in a few minutes! Once you're ready to move on, you can move on to the more in-depth documentation, as described below...

.. hint::

   The :doc:`/topics/getting-started/index` topic is the perfect way to take your first steps with Freya.

Documentation
-------------

The documentation for Freya is organized in to categories to help you find what you're looking for more easily:

* :doc:`/topics/index` -- contains guides specific to certain topics and may cover background information, explanations of design choices and approaches, and may cover multiple parts of Freya when they can be used in concert.
* :doc:`/reference/index` -- contains guides to specific libraries and components of Freya, giving a technical view of what is available from each component and how they may be used.
* :doc:`/recipes/index` -- contains guides to accomplishing specific tasks with Freya. They may range from the very simple to advanced use-cases, and may span the whole range of the Freya stack.
* :doc:`/tutorials/index` -- contains longer form guides, usually covering a complete process of building some web application, and which are likely to cover various elements of Freya along the way.

.. toctree::
   :hidden:

   self
   meta/index

.. toctree::
   :caption: Topics
   :hidden:
   :includehidden:

   Overview <topics/index>
   topics/getting-started/index.rst
   topics/design/index.rst
   topics/standards/index.rst

.. toctree::
   :caption: Reference
   :hidden:
   :includehidden:
              
   Overview <reference/index>
   reference/core/index
   reference/types/index
   reference/optics/index
   reference/routers/index
   reference/machines/index
   reference/polyfills/index
   
.. toctree::
   :caption: Recipes
   :glob:
   :hidden:
   :includehidden:

   Overview <recipes/index>
   recipes/*
      
.. toctree::
   :caption: Tutorials
   :hidden:
   :includehidden:

   Overview <tutorials/index>
