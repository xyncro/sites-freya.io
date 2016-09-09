Terminals
=========

In the :doc:`overview` section, the machine model was described as being a tree of decisions, where the leaves represented the action that should result when reaching the appropriate leaf. In the Hephaestus implementation, the leaves are referred to as terminals, although different names may be used when taling about specific machines -- in the HTTP example (and the Freya HTTP machine) they are referred to as handlers, in line with the traditional term "response handler".

As with decisions, there is slightly more to them than just a simple function.

Configuration
-------------

In :doc:`decisions`, the configuration process of decisions -- from an unconfigured decision to a decision value once supplied with configuration data -- was outlined. Terminals go through a similar process, being configured with the same configuration value before being "ready".

As with decisions, they can be thought of as a simple type (simplified):

.. code-block:: fsharp

   type Terminal<'configuration> =
       'configuration -> TerminalValue

In the case of terminals, the logic is somewhat simpler than with decisions in one sense -- the terminal is only meaningful when run, so there is no consideration of a terminal being known ahead of time.

State
-----

Also as with decisions, terminals need to act upon the input data, the state, to produce the result required. There is no need for them to produce a boolean result to determine what action to take next however -- they are by definition the last part of a machine computation.

The terminal value can be seen then approximately as being (again, simplified):

.. code-block:: fsharp

   type TerminalValue<'state> =
       | Terminal of 'state -> 'state

As you can see, the state is returned so that machines may fit in to broader computations, be composed, etc.

Results
-------

There is one addition -- the result of a machine computation. Although the computation as described may have modified (or returned a new instance) of the state provided, it currently has no responsibility to return a value. This is the responsibility of the terminal, and leads to this definition, expanded to encompass a result type (as ever, simplified):

.. code-block:: fsharp

   type TerminalValue<'result,'state> =
       | Terminal of 'state -> 'result * 'state

This now allows for a more complete definition of the terminal type also:

.. code-block:: fsharp

   type Terminal<'configuration,'result,'state> =
       'configuration -> TerminalValue<'result,'state>

This is again close enough to the meaning of the actual implementation to see the shape of a machine computation -- and to note that it can be parameterized by the types of the configuration data, the result, and the state passed through the computation.

This gives a very flexible model, and in the following sections on :doc:`optimisation` and :doc:`application` within Freya, you will see how this model allows for an efficient execution approach, and the concrete implementation of the machine model in Freya.
