# CON402 - Advanced Patterns in Microservices Implementation with Amazon ECS

* Microservices Architecture
  * architectural and organiztional approach to software development in which software is composed of small, indpendent services, that communicte over well-defined APIs. These services are owned by small, self-contained teams
    * http://bit.ly/2A0qGdt - Running Containerized microservices on AWS
* Monolithic vs Microservices
  * microservice - do one thing and do it well
  * monolithic - singular, kitchen sink, hard to change, hard to deploy

* Why microservices 
  * scaling because monoliths don't need to scale everything, micro scales only what is neeed
  * change - in monolithic applications change is hard

* Characteristics
 * decentralized
 * polyglot
   * can speak any technology
   * the owner of the team owns the technology that is used and it is thier choice
 * Independent
   * Teams are empowered to make the changes you want
 * Do one thing well
   * mirror the buisness function, but only the one that makes sense to the function and no more
   * Domain driven design is a good starting place for this
 * black box
   * each microservice is a black box to the others
   * removes coupling
   * If you open your data layer for example this can slow you down when you want to change it
 * You build it, you run it
   * The team owns the service from development to deployment
     * More ownership
     * More satisfaction
     * Devs support it as well as build it
     * code quality increases
* Why Amazon ECS
  * high performance platform for running Docker containers
  * fully manged elastic service - you don't need to run anything and services scales as your microservices architecture scales
  * shared state optimistic scheduling
  * integrates with other Amazon services like CloudWatch
  * integration with Code* services for integration and delivery (CI/CD)

* Deploying Containers on ECS
 * scheduler
   * Batch jobs
   * long running apps
     * health management
     * ECS setice scheduler
     * scale-up and scale-down
     * AZ ware
     * grouped containers)

* The twelve-factor App
  * Twelvefactor.net
  * Items
    * Codebase
    * dependencies
    * config
    * backing services
    * build, release run
    * Processes
    * Port Binding
    * concurrency
    * disposibility

* Automatic Service scaling
  * Scale across two dimensions
    * app level how many tasks are in the app
    * cluster itself - things like memory reservation and cpu resercation
* IAM roles for tasks
  * delegate permissions to a resoruce without passing keys/password
  * since containers are another compute primative you can give IAM roles to tasks
  * on the same EC2 instance you can have tasks that have different security access

* Secrets Management
  * Passwords encrypted with KMS key
  * task has IAM role that has the rule it can decrypt the value

* Continuous Deployment
* Blue-Green deployments
  * Mirror of application and then redirect traffic
  * Switch on DNS-based or at the target group level 

* Consuming events for service discovery
  * Average life expectency of containers is days and you need ways to find them
  * Service discovery with route 53 - coming soon ~Q1 next year

* nginx reverse proxy
  * http://amzn.to/2neahgV
  * can turn something that is barely web friendly and make it nice

* Custom, high resolution metrics
  * http://amzn.to/2zcr05K


=======
* Task Placement Examples
  * You can specify a specific type of task land on certain types of machines 
  * can also specify the valid/invalid availability zones
  * you can spread it across zones
  * affinity and anti-affinity
  * do it at run time when start task or in the service definition


* BuzzFeed building a platform ontop of ECS
  * Called Rig - oppinionated end to end workflow management
  * what did they learn
    * make your development and deployment workflow as frictionless as possible
      * mono repo approach taken and its benifits are outweighing the downside
      * same process applies the same for all people
    * target abstractions, force consistency
      * service defintion set
      * config sepearate - runtime agnostic 
      * higher level abstraction gives leverage to constrain the features to support but be clear what is support and actually change the back end implementation
      * Developer consistency enables flexability in the organization
      * Ops things like monitoring configuration are consistent and easy to know how to use across the stack
    * Leverage the whole AWS platform
      * Sometimes simple/basic approach is good enough and you can get a lot out of it
    * Make everyting as self service as possible
  * Challenges
   * Network and access
     * Everything runs everwhere on ECS
     * Each service can access every other service
     * Quick fix - isolate ECS clusters and VPC's
       * More overhead for operators
       * more work for end users (they shouldn't care about the details of the networks)
      * A better fix - task placement & class host groups
        * both workload based and privlege based 
          * put offline bach processes together
          * auth level issolate
    * Quickly and safely rolling clusters
      * First approach was naive - spin up a new one and let it tear down 
        * As the cluster grows this can take hours
      * A better approach have a lambda function observing the state and make sure it is safe to teardown
    * Sharing ECR registries
      * wanted multiple ECR registries in multiple regions
      * simple fix is push to all regions when doing a push
      * this is trickier when sharing across accounts
      * More sophisicated is to use a lambda function
    * Efficency
      * moving away from moving individual resources is a big efficency but there are more things
      * reservation vs utilization
        * money  being wasted 
        * effects the ability to scale
        * move from elastic load balancers to application load balancers
        * custom cloud watch metric to look at both cpu and memory at the same time
  * What's next
    * Indivual service can 
