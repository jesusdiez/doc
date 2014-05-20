# DDD Workshop @ Akamon
By @mathias_verrraes

19/05/2014


## Intro

- Everything's a System. And a part of another system
- There's no feedback if there's no change (a reaction) to the "data input".
- "Systems inherently resist chage"
- The more adapted to the environment a software is the less flexible to future changes it is.

LEAN - not build product, but a method to measure what the customer wants.

Fast iteration, but slow software feedback, anyway: DDD goes deeper than lean, agile, kanban, TDD...

**Keep Domain and Domain Model in sync**

The hard part is not the implementation, it doesn't matter too much, is how to expose my functionality, behaviour to the outside world :)

---

## Modelling

### ValueObjects and Entities

Expressing the goal of creating this object in a declarative language.

Value object: gives a meaning to something that doesn't seem to have by itself.

It should have an internal representation and an external representation.

The database could also be considered an external representation.

##### SchoolYear example
	
Normal instantation

	new Time("10:30");

Does this have value? maybe too abstract (depends on domain)
	
	new Time(new Hour(10), new Minute(30), ... ) );
	new TimeSlot(Time1, Time2);

TimeSlot would be the one validation both, not every each.

Use the correct level of abstraction. Domain representation, would be understanable from the domain. 
The complexity of using two objects of type ```Time``` is removed.

	$timeslot = TimeSlot::Parse('10:00-11:00');
	
Benefit: on the outside we're expressing explictly the domain.

The ```SchoolYear``` "calls for behaviour".

In the domain, could be 2013 Sep 1st - 2014 Aug 31st.
```SchoolYear``` is just an spceific version of a ```DateRange```.

We try to avoid injecting a service on a Domain object.


	class SchoolYear {
		private DateRange dateRange;
		
		static public function Parse();

		/** @var SchoolYear */
		static public function Next();  
	}


I.e.: we want the amount and currency to be together. We can't compare with other without both values: Money.


Entity: can evolve in time, but maintains their essence (depends on domain). Until you determine their values can change without modifing it's essential entity, try to work with it as a Value Object as much as you can.

#### Monoids

Associativity, commutativity, neutral element

[..-2, -1, 0, 1, 2, .. ] Addition and this kind of numbers, are monoids.

	$m = new Money(5, 'EUR');
	$m2 = $m->add(new Money(5, 'EUR'));
	
Their responsability is to be that thing and their essential behaviours; as soon as you start needing for something that changes, you may have an entity.
Value object or Entity: maintains their entity once it has changed (entity) or becomes a different entity (valueObject).
	
Offtopic: **Composition over inheritance**

#### Entities

Entities are things that change, sometimes they're removed from the system.

Entity has behaviour, needs to be consistent. Preferily it wouldn't allow you to modify it inconsistenly. You wouldn't change the state from the payment with an state machine but you would run a "pay" command of the entity and it would change its status if it can.


















	












	











