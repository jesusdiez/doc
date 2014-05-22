# DDD Workshop @ Akamon
By @mathias_verrraes

19/05/2013


# Day 1. 

## Intro

- Everything's a System. And a part of another system
- There's no feedback if there's no change (a reaction) to the "data input".
- "Systems inherently resist chage"
- The more adapted to the environment a software is the less flexible to future changes it is.

LEAN - not build product, but a method to measure what the customer wants.

Fast iteration, but slow software feedback, anyway: DDD goes deeper than lean, agile, kanban, TDD...

**Keep Domain and Domain Model in sync**

The hard part is not the implementation, it doesn't matter too much, is how to expose my functionality, behaviour to the outside world :)

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

---

# Day 2

## CQRS

_CQRS: Command / Query Responsibility Segregation_

Each method _should_ return something or change something, but never both.

### Concert Ticket example

Aggregrate: a wrapper around entities that defines a whole.
Consistent boundary, transactionally.

Single ticket: 3 states. [available|sold|reserved].
 : Protect environment

Distributed systems
CAP Theorem: Consistency | Availability | Partition Tolerance.
You can only get 2 of them.

Highly cohesive Read+Write model? Same entity for reading/writing something: bottleneck. 

We separate the databases in a database for writing, another for reading, that could be "Eventually consistent" (you can do some queries and, in some time, it will be consistent. It's _previously consistent_, it was consistent in a previous moment.)

**Commands** are "messages". Can fail, can be rejected. Have an intention to change something in your behavior. 
**Events** define that something had happened, and are messages again.

Read Model waits for events to happen and changes the status of something.

The client asks the Read Model some information 
**DTO**

On the write model you're guarding the invariants (maintaining the model consistant).
 
 
User commands: from a Task Based UI. Not a simple form with user data, some actions: update address, set as VIP, deactivate...

ReadModel: Polyglot Persistence.

http://www.infoq.com/presentations/cqrs-erlang

	Tasks --> Command > Queaue > Comamnd Handler > Aggegate > Event --> EVENT STORE --> Queue
	Queue ==> Event Handler -> READ STORE -> DTO --> Read Only --> Tasks





###Basket example

**Domain events are immutable**
When something has happened, it's in the past, so you can't modify it.

Data centric - event centric

We could replay all the commands sent: command sourcing: dangerous.
We replay the events generated by the commands to put the model in the exact same state.


# afternoon
- **shared kernel** 
Some model that you share between differnet bounded contexts, parts of your system

- **customer / supplier**
How the relationship works. I can ask for features but they're not guaranteed  

- **conformist**
having different point of views 

- **anti-corruption layer**
something to protect yourself from other system

- **separate ways**
something starts like a common model but it laters separates in some

- **open host service**
some service you can consume but you don't control

- **published language** 
something that is described in some document. you cna base your model on an existing language. you may not need your own ubiquitous language if it already exists.

- **big ball of mud**
spaghetti code 

### Educational example

Context map for the educational app: Disgital School Platform

* web app where teachers can login.
* Model

* WISA: pupils, staff, schools. 
takes care of what students are enlisted on what schools, what groups are in, what teachers are on each school, what subjects do they teach, what schools are on the system...
* OVSG: manages learning goals
	- Cluster learning goals
* Government
* Cluster Stemps
* STICORDI: control learning disability 


# Day 3

Parts of a model

- Operations
- Behaviours
- Invariants
- Value Objects
- Entites
- Relationships
- Factories
- Boundaries
- Commands / Intentions
- Events / History / Facts
- Messages
- User / Actor / Role
- Business rules
- Processes / Workflows
- Policies
- Time Related / Temporal
- Reports


**Make the implicit explicit**

Entity: equality by identity lifecicle. mutable

Value object: equality by value. immutable




## Books

* Leading change
* Switch! by chip and 
* Fearless change.
* How to change the world: change management 3.0
