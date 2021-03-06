# SRV322 - Migration to Serverless: Design Patterns and Best Practices

* Migration strategies
  * rehosting - lift and shift 
    * automate
        * use migration tools
    * manual
      * manual install
      * manual config
      * manual deploy
  * replatforming - lift & reshape
    * determine new platform
    * modify underling infrastructure
    * use migration tools
  * repurchasing - dorp & shop
    * purchase COTS/SaaS & licensing
    * manual install & etup
  * refactoring - rewriting/decoupling
    * redsign application/infrastructure arictecture
    * app code development
    * full ALM/SDLS
    * integration
  * retain/ not moving
  * retire/decomission
    * strategy - turn off and if someone screams
      * if someone screams they one it
      * if no one screams you can take it off

* Servless Environment
  * Lambda is a execution platform
    * it alone is not enough it needs the rest of the evnrionment
  * integrates with manged services
  * integration patterns
    * things like step functions are needed to help migrate

* Serverless migrations
  * deployment model 
    * how are you going to deploy these
    * when you have a lot of small things how you going to manage them both as deploy and overall
  * data migration
    * where is the data going to be and how to go
  * integration with landscape

* How to migrate
  * all-in/big bang
  * progressive
  * integrate and migrate
    * Try to unlock some new functionality through this
    * provide buisness value in combination of the migration

* some topics of interest
  * develop and debug
  * deploy and upgrade
  * monitor and manage


* How do you manage a fleet of functions
  * bit.ly/serverless_lens
    * https://d1.awsstatic.com/whitepapers/architecture/AWS-Serverless-Applications-Lens.pdf
  * You need to be able to automate CI/CD and if you don't have that, you need to get that capability first

* How do you know if it is good to above?
  * see above
  * You do pay for the overhead

* Think tables not databases/ think domain driven development
* https://d0.awsstatic.com/whitepapers/microservices-on-aws.pdf
* when does the boundry begin or end 
* Book - Building Microservices - https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems

* You can always ask to talk to a Solutions Architect to get help

* SAGA - microservices transaction pattern - how to rollback in transactions across 
  * use step functions

* Benchmark to find overhead for something like serverless - use something like gatling or jmeter to test this
  * Transaction Percentile - Look at TP 99 & TP 50
* think about code start optimization

* Heartbeat to keep something warm
  * probably an anti-pattern because its probably a pre-mature optimization
  * maybe a better way to do this is to batch the requests 

