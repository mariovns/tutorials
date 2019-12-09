# Microservices
Timeline ------> <p>

Mainframe computing -> client-server architecture (n-tier) -> server-oriented architecture (web-services)

Microservices
* fine grained SOA, small ephemeral components
* loosely couples SOA with bounded contexts
* modular components, called from one another
* Agility, distributability, scaleability are the deal breakers
* lightweight processes communicating over HTTP, created and deployed with relatively small effort

Monolithic architecture
* presentation, business, dao, all logic inside 1 or interdependent archives
* Benefits
	* code reusability is more
	* minimal overhead
	* easy to extend features & API
* Shortcomings where Micro-service could help out
	* scaleability not achievable
	* release rollbacks
	* tough to adopt new technologies in independent components
	* failure cascading (Circuit-breakers in ms)
	* agile task categorisation tough
	* tough maintenance & bringing up a new developer to know full application

## Microservice Design
* each component may have different database, different presentation tech
*  The API gateway can be a single incoming point for all request which redirects the request to eligible component to serve the request
*  Load Balancer can be utilized to smoothen out the large no. of request coming for a particular component by transferring it to its other replicas
*  Circuit Breaker can be employed at the initial layer to not let a failure cascade throughout the system and make the component unreachable in case of any issue with it
*  Monitoring tools can be added to control and monitor the no. of request coming for any component and the inter-component interactions

### Nanoservice - Teraservice
* Nanoservice : anti-pattern, too fine-grained so that maintenance become overheade
* Teraservices : more like monolith in itself, so can have all similar shortcomings

### Challenges
* Complexity : deployment complexity with increase of components
* Bandwidth latency (Distribution Tax) : no. of inter-component communication increases exponentially
* Flow : lots of microservices then the flow of data and understanding it can be a challenge
* Configuration : with no. of microservices, the maintenance & configuration gets exceedingly tough
* Monitoring : different microservices may need different kind of monitoring & needs to be maintained separately
* Logging : increases with no. of micro-service
* Transaction : transaction maintenance tough, rollbaclk tougher
* Automation : w/o process automation or CI/CD, micro-service based product may not be a success

### Microservices & cloud-native
* CN : cloud infra
* MS fits nicely in CN architecture
* CN can be monolithic
* CN & MS both 12-factor application development


### Domain Driven Design


## Core Concepts of Microservices
* Services
* Communication
* Distribution
* Database & Transaction
* API Layer

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

### Bounded Context
* investigate working system, determine the domains, break services accordingly
* reduce cross - domain calls
* reduces latency by limiting distribution tax
* strong contracts and well-defined boundaries allow for self-discovery (by consumer)

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

*** BOTTOMLINE *** Try to adopt devOps culture
