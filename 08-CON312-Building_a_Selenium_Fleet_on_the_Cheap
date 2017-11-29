# CON312 - Building a Selenium Fleet on the Cheap with Amazon ECS with Spot Fleet

* Previous solution - Selenium gird scaler 
  * written in java
  * Open source
  * preallocate capacity
  * spin up hubs on EC2 and hand off addresses
  * Upgrade to selenium or browser required new AMI build
* ECS Based Selenimum with Spot Fleet
  * Two clusters
    * Hub cluster
        * ELB in front of autosaling group
        * service (seperate container) for chrome and firefox
    * Node Cluster
      * Pool of boxes provisioned with spot fleet
      * same split between chrome and firefox
  * Uses docker and don't need to build a new AMI
  * Spot fleet - has some risks, but cheaper, better hardware

* Challenges
  * Node resistration to hub
    * introspect the container to find mapped port
    * Coming CLI argument to selenium to pass port dynamically 
  * Security - containers have to much access, more secure by giving only task level
  * Auto scaling responsiveness - spot fleet is slow to give instances 2 minutes to realize you need it, 8 mintues to scale up
    * avoided by just buying a bunch of instances at all times
  * Bugs
    * hub doesn't gracefully handle prolems

* Resources
  * https://github.com/RetailMeNotSandbox/ecs-selenium
  