Application
===========

In the previous sections on the design of :doc:`../machines` the principles and implementation of the general machine model were set out, as implemented in the Hephaestus machine library used by Freya. This was set out in general terms, but Freya implements a concrete machine in the form of the Freya HTTP machine.

Given the specific nature of HTTP, some of the terminology is specialised to that area (for example, terminals in the HTTP machine are referred to as handlers). The HTTP machine is also modular, with certain additions to the core HTTP standard being developed as extensions to the core machine (for example, CORS support).

Details on the Freya HTTP machine can be found in the reference section on :doc:`/reference/machines/index`, specifically in the :doc:`/reference/machines/http/index` section, with full coverage of the machine, the configuration approach, and available extensions.
