# Microservices
Timeline ------> <p>

Mainframe computing -> client-server architecture (n-tier) -> server-oriented architecture (web-services)

Microservices
* fine grained SOA, small ephemeral components
* loosely couples SOA with bounded contexts
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
* Flow : lots of microservices then the flow of data and understanding it can be a challenge
* Configuration : with no. of microservices, the maintenance & configuration gets exceedingly tough
* Monitoring : differnret microservices may need different kind of monitoring & needs to be maintained separately
* Logging : increases with no. of micro-service
* Transaction : transaction maintenance tough, rollbaclk tougher
* Automation : w/o process automation or CI/CD, micro-service based product may not be a success

### Domain Driven Design
