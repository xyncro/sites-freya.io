Model
=====

The HTTP model is built using a modular approach using a pattern implemented in the underlying `Hephaestus <https://xyncro.tech/hephasestus>`_ machine library. The Hephaestus library allows for powerful approaches to composing machines, which is out of scope for Freya documentation, but drives a useful modularity in terms of the HTTP model.

This modularity will be immediately visible in the diagrams of the HTTP machine, and reflected in the documentation around configuration of :doc:`configuration/decisions`.

Diagrams
--------

Clickable diagrams are available (these are generated from the Omnigraffle design documents for the HTTP machine and extensions, maintained as part of the `Freya Machines repository <https://github.com/xyncro/freya-machines>`_):

* `HTTP Model Diagrams <../../../_static/components.core.html>`_

Visualisation
-------------

In the :doc:`overview`, the terminology of Decisions and Handlers was defined, and these are visualised in the clickable diagram of the HTTP model. The composition elements are informative in terms of grouping and structure, but the decisions and handlers are the key elements of the model. The naming of these elements is identical to the configuration syntax that may be used in the computation expression, allowing the diagrams to be used as a direct reference along with the detailed reference on each configuration option.

Decisions
^^^^^^^^^

Decisions are represented as diamonds, with one ``true`` outcome and one ``false`` outcome. The true outcome is represented by a solid line, and the false outcome by a dashed line.

* **Decisions which may be configured directly** as part of the HTTP machine are coloured strong blue.
* **Decisions which may be influenced through configuration** but not configured directly are coloured light blue.
* **Decisions which may not be configured** are coloured grey.

There is a full reference for the HTTP machine :doc:`configuration/decisions`, as well as a full reference for the :doc:`configuration/properties` which may be configured to influence decisions which are not configured directly.

Handlers
^^^^^^^^
  
Handlers are represented by boxes, showing the HTTP Staus Code associated with the handler by default. Colour is used to represent the semantic meaning of the HTTP results returned -- green for 2xx, yellow for 3xx, orange for 4xx and dark orange for 5xx.

There is a full reference for the HTTP machine :doc:`configuration/handlers`.
