Overview
========

Introduction
------------

Machines as implemented in Freya (internally using the `Hephaestus <https://xyncro.tech/hephaestus>`_ library) are ways to program based on decisions.

Many problems (particularly problems like how to respond to an HTTP request) can be modelled in this way -- as a series of decisions (boolean -- true/false -- decisions) to be made on the way to eventually arriving at a result. You can imagine the process of responding to an HTTP request as a series of suitable decisions -- for example:

.. code-block:: text

   Is the server in a state to respond?
       true: continue
       false: return "503 Service Unavailable"
       
   Is the version of HTTP being requested supported?
       true: continue
       false: return "505 HTTP Version Not Supported"
       
   etc. until a suitable response is determined and returned.

As you can see, this is effectively a tree of decisions -- a binary tree. The machine model requires that the logic that needs to be expressed is encoded in this form -- as a tree of decisions, where the leaves represent the action that should result upon reaching the leaf.

In the case of an HTTP machine, handling an HTTP request, the endpoints would represent response handlers to be invoked, sending the chosen HTTP response back to the client.

Exemplar
--------

The HTTP problem will continue to be used in the machine design topic as the common example case. This provides concrete examples, as well as tying the topic directly to the implementation of one of the most powerful parts of the Freya stack, a full-featured HTTP machine implementation.

The next section will cover :doc:`decisions` in more detail, and begin to introduce some of the approaches taken to machines in the implementation.
