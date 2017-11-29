# ARC306 - High Resiliency & Availability of online entertainment Communities Using Multiple AWS Regions

* Highest production quality of video

* Why multi-region?
  * lower latecncy to subset of customers
  * Legal and regulatory compliance
  * Buisness continuity/disaster recovery

* Design for failure
  * Stuff breaks all the time, but rarely in multiple places at the same time
  * this is a cost driver so needs to be considered

* do you want multi-region?
  * There is inherent latency transfering between regions
  * problems may be hidden that doing this will reveal
  * most people are not testing the DR

* The four strategies
  * backup & restore
  * Pilot light
  * warm standby
  * Multisite

* What is available to go multi-region
  * VPC - multiple AZ's in each reagion
  * connect using route 53 (DNS)
  * Cross region replication for DB's
  * Ensure load balancing at acess resion
  * AMI cross region AMI copy
  * Logging an monioring 
  * Deploy (cloud formation) across region

* Loose coupling is key 
  * Use common interfaces or common API's between the components
  * build independent components, decouple interactions
  * messaging enables decoupling
    * Amazon Simple notifacation services (SNS)
      * scales
      * simple apis
      * stored across multiple AZ's
    * Amazon Simple queuing services
      * redundent across multiple AZ's
      * messages stored up to 14 days
      * scales without pre-provisioning

* what does multi-region mean - key definition?
  * What it means will determine what your strategy is
  * what is the architecture for infrastructure
    * What is the scope of infrastructure as a service
    * How are users going to be routed?
      * User affinity
      * minimize downtime
    * How is this going to be monitored/logged?
      * Do you keep the tool for monitoring aross regions (probably not)
  * what is the architecture for applications
  * how are you going to test it
  * how do you transtion?


* Questions when going multiregion
  * does data need to be in sync in all regions?
  * how quickly does the data have to be in-sync?
  * does our technical stack already support multi-regions
  * how do you perform a seamless rollout
