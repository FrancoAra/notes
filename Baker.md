# Baker

We need to make a strong decoupling between baker's domain logic and execution logic, between "what" baker does, and "how" it does it, 
between the baker issues, Akka issues and distributed systems issues.

By achieving this, we can state the properties we want to guarantee from each layer.

____________________________________________________________________________________________________________________________________________________
## Baker

### Core Axioms
* Users have several services which specialise on specific business sub-processes.
* Users achieve value by composing and orchestrating correctly and with ease such individual services.
* Each service is unreliable and might behave erratically.

### Objects 
* _Recipe_: DSL for expressing service orchestration processes.
* _Event_: Part of the DSL, contains an event name, and possible data to be processed by a process instance.
* _Process Instance_: Instance of a recipe that will track and execute the orchestration for specific input events.
* _Recipe Manager_: Provides storage and accessibility of available recipes.
* _Process Index_: Gateway for communication with living processes.
* _Calling Process_: the process that holds a process id, and therefore is able to send messages to a process instance through the process index

### Properties
* _


___________________________________________________________________________________________________________________________________________________
## Distribution

### Core Axioms
* We require distribution to offer availability and reliability.

### Objects
* _Node_:  Entity which contains the whole Baker protocol, it can communicate with other peer nodes, and might die or lose communication randomly.
* _Cluster_: Group of nodes distributed over data centres.
* _Journal_: Store of events that have occurred at a point of time to the system, which by orderly reproduction, can restore state.
* _Event Bus_: Publish/Subscribe channel of events.
* _Stateful Entity_: Part of the system with transactional access to a private peace of memory, computing capabilities, reacts to messages.
* _EntityRef_: Reference to a stateful entity, can be used to 
* _Message_: Data directed to a stateful entity.
* _Event_: Categorised data which can be sent to an event bus, and received from an event bus.

### Properties
* _Self Healing Event Sourcing Restore_: A stateful entity can be reinitialized after failure and restores state by replaying events from a journal.
* _Split Brain Resolution_: If data centres between nodes lose communication, the core functionality preserves, and when communication restores, a unified state is reached again.
* _Cluster Sharded_: A stateful entity is ensured to have a single instance at any time, location in the cluster is not predefined and on failure detection self healing event sourcing restore is triggered

### Feature
1. Every _stateful entity_ has _self healing event sourcing_ capabilities.
4. The _cluster_ has _split brain resolution_ capabilities.


___________________________________________________________________________________________________________________________________________________
## Implementation
... Akka ...


___________________________________________________________________________________________________________________________________________________
## Development

* _Type Safety_: Quick test, try to find code that if you delete, it will still compile but will generate runtime errors.
* _Domain Decoupling_: There is a clear separation between WHAT baker does and HOW it does it.
* _Modularity_: Quick test, how much code do I need to change in order to change a functionality? Is it decoupled? Can I easily exchange to different implementations?
* _Testing ease_: Quick test, how much code do I need to write to make a full functional test of a feature? Is it only the domain logic statements, or also technical implementations?
* _Test decoupling_: There are tests for domain logic which does not require the production implementation, and there are tests which test the production implementation independently.
* _Test completeness_: Tests do reflect the formal and semi-formal specification of the system.