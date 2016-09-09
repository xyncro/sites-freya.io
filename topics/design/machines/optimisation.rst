Optimisation
============

In the :doc:`overview` the basic premise of machine computation was introduced, followed by the definition of :doc:`decisions` in the next section. One of the goals of the machine computation model, as expressed in Hephaestus, is to make the eventual runtime computation as efficient as possible.

In the section on decisions, the definition of a decision was declared to be approximately as follows:

.. code-block:: fsharp

   type DecisionValue<'state> =
       | Function of ('state -> bool * 'state)
       | Literal of bool

This definition leads to the possibility of significant optimisation of the resulting decision tree structure.

Reachability
------------

The simple but effective approach taken is based on the premise that where a decision is a literal, it is already possible to tell which subsequent branch of the tree is reachable, and which is not. Wherever a decision is a literal, it is possible to *prune* the decision tree, effectively removing the decision entirely, and creating a new tree where the execution path will go directly to the appropriate sub-tree.

Hephaestus trees are not defined to be pure trees (branches can join, so two paths can lead to the same decision). This requires a slightly more complex algorithm to safely prune the tree structure, but once it is pruned the tree becomes a tree where all remaining decisions are functions and not literals.

This can have significant impact on performance and complexity of the eventual decision tree. The Freya HTTP machine in the default pre-configured, pre-optimisation stage, has several hundred decisions, forming a large and complex tree. If given minimal configuration (which will result in many of these decisions assuming default, literal values) this number goes down to around five decisions in total!

As configuration becomes more complex, this number begins to rise, but the resulting tree is always significantly simpler than the potential maximal tree.

Implications
------------

The implications of optimising the decision tree in this way on program design are worth noting.

First, in the overall design of the tree, wholesale duplication of logic can be permitted, as any unneeded logical paths will be removed before runtime use by the configuration and optimisation process.

Second, the expectation of what will be run becomes less important -- as sections of logic may be optimised away, the expectation shifts to only running what is relevant, leading to thinking more declaratively about the overall logic. Interactions between logical elements are reduced in importance, and the importance of simple logical decisions comes to the fore, emphasising the decision based nature of the machine approach further.

The next section on :doc:`application` gives some detail on the adoption of the machine model within Freya.
