Model
=====

.. important::

   This documentation is currently in progress. While it may be useful, it is likely to undergo change and improvement. Please check regularly. Documentation updates are also announced on the `Freya Twitter feed <https://twitter.com/freyafs>`_ -- follow Freya there for up to the minute information on changes to this documentation, and other Freya news.

The HTTP model is built using a modular approach using a pattern implemented in the underlying `Hephaestus <https://xyncro.tech/hephasestus>`_ machine library. The Hephaestus library allows for powerful approaches to composing machines, which is out of scope for Freya documentation, but drives a useful modularity in terms of the HTTP model.

Elements
--------

In the :doc:`overview`, the terminology of Decisions and Handlers was confirmed, and these are visualised in the interactive diagram of the HTTP model. The composition elements are informative in terms of grouping and structure, but the decisions and handlers are the key elements of the model.

Decisions are represented as diamonds, where the True path is represented by a solid line, and the False path by a dashed line. Handlers are represented by boxes, showing the HTTP Status and Reason associated with the handler.

Diagrams
--------

Clickable HTML image maps are available:

.. raw:: html
         
   <a href="../../../_static/components.core.html" target="_blank">Diagrams</a>
