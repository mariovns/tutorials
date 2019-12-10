# Microservices
In the quest of finding better ways to build systems, Microservices have emerged from below requirements
* Domain-driven design
* Continuous delivery
* On-demand virtualization
* Infrastructure automation
* Small autonomous teams
* Systems at scale

## Timeline ------> <p>

Mainframe computing -> client-server architecture (n-tier) -> server-oriented architecture (web-services)

### Monolithic architecture
* presentation, business, dao, all logic inside 1 or interdependent archives
* Benefits
	* code reusability is more
	* minimal overhead
	* easy to extend features & API
* Shortcomings
	* scaleability not achievable
	* release rollbacks
	* tough to adopt new technologies in independent components
	* failure cascading 
	* agile task categorisation tough
	* tough maintenance & bringing up a new developer to know full application

### Service-oriented architecture
Service-oriented architecture (SOA) is a design approach where multiple services collaborate to provide some end set of capabilities.
* promote the reusability of software
* easier to maintain or rewrite software
* 
  
### Microservices
  * fine grained SOA, small ephemeral components
  * loosely couples SOA with bounded contexts
  * modular components, called from one another
  * Agility, distributability, scaleability are the deal breakers
  * lightweight processes communicating over HTTP, created and deployed with relatively small effort

### Definition
Microservices are small, autonomous services that work together. It can further be broken down as : 
* Microservices take Single Responsibility Principle to independent services on business boundaries, making it obvious where code lives for a given piece of functionality.
  Smaller the services become, the no. of services & hence microservices complexity increases.
* Microservices should be autonomous and should be deployed without making changes in any other services.
* Benefits
    * Heterogenous technologies : embracing new tech quick but may be complex
    * resilience : failure doesn't cascade
    * scaling : scaled up/down as needed
    * ease of deployment
    * composability : fine-grained
    * organisational alignment : smaller focussed team 
    * replaceability : small components can be replaced easily
* Challenges
	* Complexity : deployment complexity with increase of components
	* Bandwidth latency (Distribution Tax) : no. of inter-component communication increases exponentially
	* Flow : lots of microservices then the flow of data and understanding it can be a challenge
	* Configuration : with no. of microservices, the maintenance & configuration gets exceedingly tough
	* Monitoring : different microservices may need different kind of monitoring & needs to be maintained separately
	* Logging : increases with no. of micro-service
	* Transaction : transaction maintenance tough, rollback tougher
	* Automation : w/o process automation or CI/CD, micro-service based product may not be a success
	* Issue of distributed System comes up as well, which are defined in CAP theorem.

### Microservices Add-Ons
Some additional attributes which has been added as we go along the implementation of microservice are : 
* Each component may have different database, different presentation tech
* The API gateway can be a single incoming point for all request which redirects the request to eligible component to serve the request
* Load Balancer can be utilised to smoothen out the large no. of request coming for a particular component by transferring it to its other replicas
* Circuit Breaker can be employed at the initial layer to not let a failure cascade throughout the system and make the component unreachable in case of any issue with it
* Monitoring tools can be added to control and monitor the no. of request coming for any component and the inter-component interactions
* Nanoservice : anti-pattern, too fine-grained so that maintenance become overhead
* Teraservices : more like monolith in itself, so can have all similar shortcomings


## Core Concepts of Microservices

### CAP Theorem
In a distributed environment, an application cannot have more than 2 below qualities together
* ***C***onsistency
* ***A***vailability
* ***P***artition Tolerance

It means if a patrition failure happens on an incoming request, then you can either provide consistency (by returning error response)
or provide availability(by returning corrupted response) but not both.

### Microservices & cloud-native
* CN : cloud infra
* MS fits nicely in CN architecture
* CN can be monolithic
* CN & MS both 12-factor application development

### Domain Driven Design & Bounded Context
* BC can be understood as specific responsibility enforced by explicit boundaries.
* BC should be highly cohesive but very loosely coupled with other contexts
* BC can be think of an animal cell
* BC can be clubbed according to likewise business capabilities
* investigate working system, determine the domains, break services accordingly
* reduce cross - domain calls
* reduces latency by limiting distribution tax
* strong contracts and well-defined boundaries allow for self-discovery (by consumer)

### Services
 * Communicate via Rest over HTTP
 * Common documentation measure should be adopted
 * Service discovery
 * handles 1 set of related function w/o any cross domain operations (Domain Driven Design)
 * operates on 1 defined domain, not too fine grained or not fine-grained enough

 ### Communication
 * Protocol-Aware Heterogenous Interoperability
 * Since Rest is language agnostic so each component can be written natively & so agile
 * each service is can call any other service, so orchestration is key
 * passiveness is required in service APIs
 * loosely coupling of service can cause failable changes to seep-in quite easily

 ### Distribution
 * Since services are executed through rest, renders it to be placed locally or anywhere around the globe
 * customer-facing applications to be regionally or globally available, high availability
 * business facing apps that should be available globally, whenever business expands

 ### Scaleability
 * each application is different to other applications
 * some service can take more load than others, so the instances of these service can be increased using a good API proxy layer
 * furthermore, instances can be dynamically inc or dec as the  system load varies

 ### Latency & gridlocks
 * the latency between service calls can become exponentially exaggerated if multiple inter-service calls are required to achieve 1 operation
 * delays can increase, gridlock & hence catastrophe can happen
 * if there is a circular call stack, S1->S2->S1, then problem becomes much severe
 * in case of higher latency, circuit breakers can be utilised to timeout instead of a complete system failure

### Transaction
* ACID works best in single context
* distributed transaction is tough in SOA & don't exist in MS
* in ms, we strive for eventual consistency
* ACID transaction is required in ms for some critical transaction like payments
* 

### API Layer
* don't make any transformative changes & execution in Api Layer
* a service proxy, which abstracts
* provides a stdized proxy expose only configured endpoints
* direct benefits
    * scaling
    * isolation for change
    * versioning operations

## Advanced Concepts
* Asynchronous Communication
* Tracing  & Logging
* Continuous Delivery
* Hybrid Architectures

### Asynchronous Communication
* improves system health
* event-driven ms 
    * service put msg in async msg broker & drive events
    * downstream processor will then operate on them
* stream data platforms 
    * msg written in central msg broker
    * msg broker will then call apt listeners
    * better as 1 msg can trigger multiple events
* move foor load reduction
* handling errors must be solid & recovery should be in place
* data should be routinely monitored in autoamated fashion

### Logging & Tracing
* plan for unified logging strategies across your entire platform
    * day to day operations
    * troubleshooting
    * maintenance
    * investigation
    * general tasks
* Problems
    * larger volume of artifacts
    * agile nature
    * different teams - different strategies
* Log Aggregators : help in coalesce & scanning logs efficiently
* Tracing : determine the actual flow in the system
* unique token 'trace' is made which is passed to each service-calls & logged along with the messages
* 

### Continuous Delivery
* automated way of build-deploy model (CI/CD pipeline) req for ms agility
* build-test-deploy is  the most common automated steps, but can be enhanced
* microservices priority
* 

### Hybrid Architectures

* Hierarchical service architecture
    * prevents circular dependencies
    * n - tier architecture with service
        * edge-service on top of business process service which is on top of data service
        * edge : exposes functionality to outer service
        * business process : contains actual business logic
        * gateway service : make use of business process service to configure the services exposure
        * data service : dao layer
        * adds latency to a service call
        * can introduce multiple boiler-plate stuff introducing latency
* Service Based Architecture
    * single database
    * service decomposes the business model
    * easy to model and start working with
    * with time it grows just like monolith, and gets un-manageable
    

## Architecture Choices
* Design Consideration
* Managing trade-offs
* Edge Services
* DevOps Culture

### Design Consideration
It should be done in the order & priority
* plan out CI/CD pipeline
* planning out logging and tracing frameworks
* analysis for bounded context of services for the application
* non-blocking code
* stdize your stack
* moving from sync to asynchronous service 
* more about infra & configuration

### Tradeoffs
* Distribution Tax : distributability, service-boundaries, scaleability
* Complexity : scaleability & deployment, individual services, 
* Polyglot dev process : 

### Edge services
* In bound ES & Out bound ES
* change in software requirements
* third party interactions & their changes 
* single source for changes

## Building Microservices

### Modeling Services
* The services should be having their well defined bounded context
* services can have explicit interface which decide what to share with other services
* even though deciding BC is the early requirement to a microservice designing, but deciding it prior to having a working application is difficult
* BC & data models should be different & should not be confused with
* we should divide into larger coarse grained contexts and then look if further dividing these context would help?
	the organisational/team structure can guide the solution, also keep in mind higher the services higher will be complexity.
* communication between different services can be guided by real world transactions between service entities
* technological based BC in services haven't proved to be fruitful

*** BOTTOMLINE *** Try to adopt devOps culture
