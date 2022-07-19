# selenium

# Architecture
* [grid architecture](https://www.selenium.dev/documentation/grid/architecture/)

## Components
* Event Bus
Used for sending messages which may be received asynchronously between the other components.
* Session Queue
Maintains a list of incoming sessions which have yet to be assigned to a Node by the Distributor.
* Distributor
Responsible for maintaining a model of the available locations in the Grid where a 
session may run (known as "slots") and taking any incoming new session requests and assigning them to a slot.
* Node
Runs a WebDriver session. Each session is assigned to a slot, and each node has one or more slots.
* Session Map
Maintains a mapping between the session ID and the address of the Node the session is running on.
* Router
Acts as the front-end of the Grid. This is the only part of the Grid which _may_ be exposed to the 
wider Web (though we strongly caution against it). This routes incoming requests to either the 
New Session Queue or the Node on which the session is running.

## Concepts
* slot
A slot is the place where a session can run.
* stereotype
Each slot has a stereotype. This is the minimal set of capabilities that a new session session request must match before the Distributor will send that request to the Node owning the slot.
* grid model
The Grid Model is how the Distributor tracks the state of the Grid. As the name suggests, this may sometimes fall out of sync with reality (perhaps because the Distributor has only just started). It is used in preference to querying each Node so that the Distributor can quickly assign a slot to a New Session request.

