# What is an Anoma application?

An Anoma _application_ is a set of resource kinds and solvers. Resource kinds allow to narrow down the fragments of state that are of interest to an application; then, the solvers of an application operate on (this fragment of) state, which typically involves combining user intents into transaction objects. Thus, solvers are what makes the intent-machine work for all users.  In fact, there is potentially a whole system of applications as  applications may interact (directly) with each other whenever they operate on resources of the same kind.&#x20;

### Resource kind

The resource kind fixes the resource logic and additional information that every resource of this kind carries; in this way, set of standard resource logics can be re-used for different resource kinds with similar behaviour.

### Solver

A solver is a specific user that is fabricating transaction objects from user intents such that the preferences for state (change) of intents are effected by state updates.

### Application read interface: projection functions

Projection functions constitute the application read interface: solvers use projection functions to read relevant fragments of state kept by the Anoma instance.&#x20;

### Application write interface: transaction functions

Transaction functions constitute the application write interface: solvers use transaction functions to update the state that is maintained by the Anoma instance.&#x20;
