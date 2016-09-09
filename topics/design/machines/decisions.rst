Decisions
=========

In the :doc:`overview` of machine design, the concept of a tree of decisions was introduced as being the core of the machine computational model. In the implementation -- Hephasestus -- used by Freya, that is implemented in a very specific way.

Configuration
-------------

When defining a machine, Hephaestus does not expect to be given a ready-made decision, it expects to receive a function which will return a decision. This function takes an instance of a configuration type and returns a decision, enabling for runtime configuration of each decision. In Hephaestus terms, the decision tree initially is a prototype, and will only become a tree of actual decisions once it has been configured with appropriate data.

In effect, a decision is effectively defined as the following type (this is a simplification):

.. code-block:: fsharp

   type Decision<'configuration> =
       'configuration -> DecisionValue

It may not be immediately obvious why this is useful, but it begins to become clearer when a decision value is explored.

Values
------

The act of configuring a decision, resulting in a decision value, allows for some significant optimisations to be made if a decision value is defined in a certain way.

Returning to the example of handling an HTTP request, consider a case with the following decision:

.. code-block:: text

   Is the URI too long?
       true: return "414 URI Too Long"
       false: continue

If the implementation of that decision is to compare the length of the request URI against a maximum length given in the configuration, it's possible that the result of the decision is already known. For example, if the maximum allowed length of a URI configured was *"infinite"* then the result of the decision will always be false. If the maximum allowed length was zero, then the result will always be true!

This applies widely -- many decisions are based on information provided as part of configuration, and a significant subset can be known ahead of running the machine.

For this reason, a decision value is effectively defined as the following type: (again, a simplification):

.. code-block:: fsharp

   type DecisionValue =
       | Function of // ...function returning bool
       | Literal of bool

In this way, after configuration the decision tree is a tree of decision values, where the decision value is either a function to be evaluated at runtime to give a result, or a known result. This property will be extremely useful later in :doc:`optimisation`.

State
-----

In the :doc:`overview`, you saw the example decision:

.. code-block:: text

   Is the version of HTTP being requested supported?
       true: continue
       false: return "505 HTTP Version Not Supported"

It's clear that to implement that decision, the decision will need access to the request. This is accomplished in machines by passing the state to a machine to run it. So in the HTTP example, the request state might be passed to the first decision, so that it can return a result.

In fact, in Hephaestus, the decision actually returns a result -- and a new state, which will be passed to the next decision. This allows decisions to both read relevant data, and potentially change the state for the following decisions. This is not commonly used, but can allow for more complex logic to be expressed.

This gives a slighly more realistic definition for the decision value type (again, simplified):

.. code-block:: fsharp

   type DecisionValue<'state> =
       | Function of ('state -> bool * 'state)
       | Literal of bool

In this way, it is also possible to more accurately define the original decision type:

.. code-block:: fsharp

   type Decision<'configuration,'state> =
       'configuration -> DecisionValue<'state>

This is now close enough to the actual implementation to understand the basic machine decision model, and the fundamental approach to configuring decisions.

In the next section on :doc:`terminals`, the computational model will take shape further.

