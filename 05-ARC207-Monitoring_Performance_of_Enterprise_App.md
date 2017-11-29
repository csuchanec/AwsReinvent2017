# ARC207 - Monitoring Performance of Enterprise Applications on AWS: Understanding the Dynamic Nature of Cloud Computing

* Most of the time vs when it needs to work
* Visibility
  * Average performance doesn't matter - what user expecations are and how it matches does
  * You need the detail of all the transactions and every point of the transaction so you know
  * Performance data often outstrips buisness data
  * all different types:
    * Server
    * Apps
    * User Experience
    * Buisness outcomes
  * this presents a big data problem
  * you need to know what happened when a problem occured. You can know what happened if you can't visualize what happened
  * CloudWatch - gives low-level monitoring but is insufficent
  * W

* Dynamic cloud - helps you keep running at scal
  * Two approaches
    * Better data center
      * Defined by?
        * Resources are allocated to uses, just like in a data center
        * Provisioning process is faster
        * lifetime of components is relatively long
        * Traditional capacity planning still important and still applies
      * Why use this?
        * Add new capacity faster
        * Improves availability (redundency)
        * Capacity where you need it (complaiant)
    * Dynamic cloud
      * Defined by:
        * Use only the resources you need (when they are needed)
        * Allocate/deallocate resoucres on the fly
        * Allocation/deallocation is an integral part of the application architecture
      * Enables
        * easier scaling
        * faster change
        * faster response
        * creates better 

* How to do you monitor a dynamic application
  * New piece to monitor - provisioning?
  * 

* Migration - how do you get it to the cloud
  * Its easy to not realize the gains from the cloud
  * Progressions in cloud addoption
    * Experiment - "What is this cloud thing?" 
      * a couple teams try it out
      * no "policies" implemented
      * seeing what it is about
      * Non-evasive, safe technologies
    * Policies - "Can we trust the cloud?"
      * IAM (credenctials)
      * VPC (secure network)
      * AWS Direct Connect (just another data center)
      * Clound policies begin to be formed
      * All parts of the company are involved
      * critical evolution point
    * Enable Servers - "This cloud seems to work pretty well"
      * Lift and shift
      * often fails here because people don't think about how to optimize as part of the shift
      * Paying for the advantages of the cloud but not using them
      * enable servers, enable SaaS
      * Multiple AZs/regions
        * part of a multi data center reslilency strategy
      * Independenctly: SaaS usage increases
        * non-critical or internal uses first
      * Amazon EC2
    * Enable Value Added Services - Dynamic cloud becomes a thing
      *  Managed databases
        * Amazon relational database service, Amazon Aurora
        * other managed services
          * AWS Elastic Beanstalk
    * Enable unique Services - Dynamic cloud is deeply ingrained
      * High-value, cloud-specific services
        * AWS Lambda, Amazon Kinesis
        * Amazon DynamoDB
        * Amazon Simple worfklow service, Amazon elastic transcoder
        * Amazon redshif
      * point of commitment
        * become dependent on the cloud
    * Mandate cloud Usage - Why do we need our own data centers
      * Cloud as data center replacement
      * company is now "all in" with cloud


* Cloud Adoption strategies - applications go through similar steps
* Ways this happens
  * Step #1 - non-critical / internal applications - quickly commit the application to the cloud
  * Step #2 - new applications - less aggresive in adopting cloud, but further
  * Step #3 - Application rewrites - Even less agressive
  * Step #4 - Critical applications - concervetive move to the cloud

* How to be successful
  * Understand your culture
    * moving to the cloud is a cult
  * understand your needs
  * create a solid plan
  * drive cultural change
  * monitor the change
    * monitor your adoption, start monitoring before you move to the cloud
    * During the migration, use the pre migraition data to determine what varation is going on while migrating
    * Continue monitoring after the shift
    * Monitor during all phases

* Monitor your application and infrastructure
 * Old school - all you need is to monitor the machine (CPU/memory)
   * This does not work so well anymore in dynamic
 * Need full stack monitoring to truly understand it
 




