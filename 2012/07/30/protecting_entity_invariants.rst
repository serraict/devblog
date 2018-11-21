.. _protecting_entity_invariants:

Protecting Entity Invariants
============================

In the previous post :ref:`command_validation`, 
we validated a ``RemoveItemsFromInventory`` command.
All wasn't fine yet, because we found a scenario
where this command validation alone isn't enough.
If we have one item in stock,
two users both want to remove one item
and their ``RemoveItemsFromInventory`` command is validated against 
the read model with ``CurrentCount == 1``. 
Both commands will validate, but clearly only one should succeed.
Validation alone is not sufficient here.

The invariant "cannot have less than 0 items in stock"
should be protected by the ``InventoryItem`` domain entity.
Let's put that in the form of a specification:

    *Given* an ``InventoryItem`` with events
      ``InventoryItemCreated`` and ``2 ItemsCheckedInToInventory``
    *when*
      we ``Remove 3 units`` 
    *then* we expect
      an ``InvalidOperationException: "only 2 items in stock, cannot remove 3 items"``


How to put this in code?
      
.. more::

We could code the specification as follows using ``Simple.Testing`` [#SimpleTesting]_::

    var spec = new FailingSpecification<Given<InventoryItem>, InvalidOperationException>
    {
        Guid id = Guid.NewGuid();

        return new FailingSpecification<Given<InventoryItem>, InvalidOperationException>
        {
            On = () => new Given<InventoryItem>(new Event[]
                    {
                        new InventoryItemCreated(id, "the name"),
                        new ItemsCheckedInToInventory(id, 2)
                    }),
            When = given => given.Sut.Remove(3),
            Expect =
                    {
                        ex => ex.Message == "only 2 items in stock, cannot remove 3 items"
                    },
        };
    };
    
Our domain model does not have any getters or setters:
we can only manipulate it by calling methods on it.
Each method protects it's object's invariants - 
if a variant cannot be upheld, the method throws an exception::

    InventoryItem : AggregateRoot
    {
        public void Remove(int count)
        {
            // other guard methods
            if (count > _count)
                throw new InvalidOperationException(string.Format("only {0} items in stock, cannot remove {1} items", _count, count));
            
            // apply the event
            ApplyChange(new ItemsRemovedFromInventory(_id, count));
        }
        // snip
    }

The current number of inventory items is kept in the ``_count`` field
and we use that field to validate the requested number of items to remove.
If this does not throw, we apply ``ItemsRemovedFromInventory`` event.

Intermezzo: Event Sourcing
--------------------------
    
Note that we don't simply set the ``_count`` field here.
We use event sourcing [#FowlerEventSourcing]_ [#YoungEvenSourcing]_
to keep track of all changes to our entity
and reconstruct our current state by replaying all our events.
The ``ApplyChanges`` method (from the ``AggregateRoot`` base class)
looks for a method with signature ``Apply(ItemsRemovedFromInventory e)``
and invokes it with our ``ItemsRemovedFromInventory``::

    private void Apply(ItemsRemovedFromInventory e)
    {
       _count -= e.Count;
    }

It's in this ``Apply`` method that ``_count`` is changed.
Later, we can call this method with our persisted domain events 
to reconstruct the object state at any given time.

Back to the test ...
--------------------

The output of our tests is (almost) human-readable::

    *** SPECIFICATION: when_removing_more_items_then_are_available PASSED 

    ON:
    --
    Given: SimpleCQRS.InventoryItem
     Initially:
        SimpleCQRS.InventoryItemCreated
        SimpleCQRS.ItemsCheckedInToInventory
        -
     Uncommitted events:
        -

    Results with:
    System.InvalidOperationException
    only 2 items in stock, cannot remove 3 items

    EXPECTATIONS:
    -------------
    The Invalid Operation Exception's Message must be equal to "only 2 items 
    in stock, cannot remove 3 items" PASSED

Although readability can be improved, it already
makes a nice piece of documentation of our domain entities.

Take aways:

 * Domain rules should be implemented in your entities as well as
   your validation code
 * Guarantee invariants of your entities by throwing exceptions 
   from guard functions of your domain entities
 * Write specification-like tests 


.. rubric:: Footnotes

.. [#FowlerEventSourcing] `Fowler on event souring <http://martinfowler.com/eaaDev/EventSourcing.html>`_
.. [#YoungEvenSourcing] `Greg Young on event sourcing <http://codebetter.com/gregyoung/2010/02/20/why-use-event-sourcing/>`_
.. [#SimpleTesting] `Simple.Testing NuGet package <http://nuget.org/packages/Simple.Testing>`_

.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::
