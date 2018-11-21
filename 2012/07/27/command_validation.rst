.. _command_validation:

Command Validation
==================

In :ref:`extending_greg_young_s_simplest_possible_thing` 
I thought up a couple of functional requirements.
The first functional requirements were: 

 * As a user, I should get a warning 
   when I try to remove or add a negative number of items.
   `#2 <https://github.com/serraict/m-r/issues/2>`_
 * As a user, I should get a warning 
   when I try to remove more items than are currently in stock.
   `#2 <https://github.com/serraict/m-r/issues/2>`_
 * As a user I should *not* be able 
   to remove more items from inventory than are currently in stock.
   `#1 <https://github.com/serraict/m-r/issues/1>`_

(I've added Github issue numbers so you can easily check the changes on Github)

.. more::

Positive numbers, please
------------------------

Let's start with validating a ``RemoveItemsFromInventory`` command,
against the simple rule that ``Number`` should be greater than 0.
This is how a ``RemoveItemsFromInventory`` command is issued
from the ``HomeController``::

    [HttpPost]
    public ActionResult Remove(Guid id, int number, int version)
    {
        _bus.Send(new RemoveItemsFromInventory(id, number, version));
        return RedirectToAction("Index");
    }

No validation there yet.
I choose to add server side validation,
using this validation method on the ``HomeController``::

    private void ValidateForRemoval(int numberToRemove)
    {
        if(numberToRemove <= 0 )
            ModelState.AddModelError("Number", "Should be greater than 0.");
    }

And then add a call to ``ValidateForRemoval(...)`` to the ``Remove(...)``
action::
    
    [HttpPost]
    public ActionResult Remove(Guid id, int number, int version)
    {
        ValidateForRemoval(number);

        if(!ModelState.IsValid)
        {
            ViewData.Model = _readmodel.GetInventoryItemDetails(id);
            return View();
        }

        _bus.Send(new RemoveItemsFromInventory(id, number, version));
        return RedirectToAction("Index");
    }    
    
The validation method for a ``CheckInItemsToInventory`` looks pretty similar.

We don't have that many items in stock
--------------------------------------

How would we validate a ``RemoveItemsFromInventory`` command 
if we don't have access to the domain object?
Well, we can use the read model for validation, let's add it to the validation method::

    private void ValidateForRemoval(InventoryItemDetailsDto item, int numberToRemove)
    {
        // ... other validation
        if(numberToRemove > item.CurrentCount)
            ModelState.AddModelError("Number", 
                                     "You cannot check out more items than currently are in stock.");
    }

And pass the read model ``InventoryItemDetailsDto`` to the validation method::  
    
    [HttpPost]
    public ActionResult Remove(Guid id, int number, int version)
    {
        var model = _readmodel.GetInventoryItemDetails(id);
        ValidateForRemoval(model, number);
        // etc.
    }

Because the read model is *eventually* consistent,
it's possible that we're validating against stale data here.
From a user perspective however, this is still useful validation
and it will prevent many invalid commands being passed to the domain objects.

It is simple to imagine a scenario however,
where this command validation alone isn't enough.
Imagine we have one item in stock. Two users both want to remove one item
and their ``RemoveItemsFromInventory`` command is validated against 
the read model with ``CurrentCount == 1``. 
Both commands will validate, but clearly only one should succeed.
Validation alone is not sufficient here.

The functional requirement "As a user I should not be able 
to remove more items from inventory than are currently in stock." 
should be translated into an invariant protected by a domain entity.
That will be the subject of the :ref:`next post <protecting_entity_invariants>`.
    
.. author:: default
.. categories:: none
.. tags:: CQRS
.. comments::
