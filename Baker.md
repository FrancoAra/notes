# Baker

We need to make a strong decoupling between baker's domain logic and execution logic, between "what" baker does, and "how" it does it, 
between the baker issues, Akka issues and distributed systems issues.

By achieving this, we can state the properties we want to guarantee from each layer.

## Baker Features

### Core Axioms
* Users have several services which specialise on specific business sub-processes.
* Users achieve value by composing and orchestrating correctly and with ease such individual services.
* Each service is unreliable and might behave erratically.

### Objects 
* _Recipe:_ DSL for expressing service orchestration processes.
* _Process:_ Instance of a recipe that will track and execute the orchestration for specific input events.
* _Recipe Manager:_ Provides storage and accessibility of available recipes.
* _Process Index:_ Gateway for communication with living processes.

### Properties
* [Recipe And Process](./recipe-and-process-properties.md)


## Baker: Process

## Implementation Features

### Core Axioms
* We require distribution to offer availability and reliability.

### Objects
* _Node_:  Entity which contains the whole Baker protocol, it can communicate with other peer nodes, and might die or lose communication randomly.
* _Cluster_: Group of nodes distributed over data centres.
* _Journal_: Store of events that have occurred at a point of time to the system, which by orderly reproduction, can restore state.
* _Event Bus_: Publish/Subscribe channel of events.

### Properties
* _Self Healing Event Sourcing Restore:_ Every stateful part of the system automatically restores state after any random crash.
* _Split Brain Resolve:_ If data centres between nodes lose communication, the core functionality preserves, and when communication restores, a unified state is reached again.

## Development Features

* _Type Safety:_ Quick test, try to find code that if you delete, it will still compile but will generate runtime errors.
* _Modularity:_ Quick test, how much code do I need to change in order to change a functionality? Is it decoupled? Can I easily exchange to different implementations?
* _Testing ease:_ Quick test, how much code do I need to write to make a full functional test of a feature? Is it only the domain logic statements, or also technical implementations?
* _Test decoupling:_ There are tests for domain logic which does not require the production implementation, and there are tests which test the production implementation independently.
* _Test completeness:_ Tests do reflect the formal and semi-formal specification of the system.