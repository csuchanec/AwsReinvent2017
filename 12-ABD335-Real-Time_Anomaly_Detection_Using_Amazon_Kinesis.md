# ABD335 Real-Time Anomaly Detection Using Amazon Kinesis

* Streaming data vs batch data
  * Streaming produced continiously
    * When captured, processed, and reacted continiously in near real time
  * Batch - you do on cadience, not real time

* Value getting more value out of your data faster
* Example - Fraud detection
  * timely and more valuable if credit card flags on the first instance of an anomylsis 

* Extract value out of data quickly

* Key requirements for processing real-time, streaming data
  * Durability - 
    * reduce the amount of data loss (don't have to throw away things like logs)
  * Contininious
  * Fast
    * Speed of capture and processing 
  * Correct
  * Reactive
  * Reliable

* Kinesis
  * Amazon Kinesis Data Streams
    * build custom applications that process and analyze streaming data
    * Fundemental infrastructure at AWS (Amazon uses it)
    * Details
      * Create shards put records and write to the streams
      * Amazon SDK's, 3rd party, log loaders implemented to make it
      * Easy configuration in a lot of systems to use the stream
      * Kinesis SDK - some simple things that are powerful 
      * Well integrated into the AWS systems to produce and use 
      * Low cost to own 
      * Not autoscaled
      * You set up number of shards and its all configuration
      * Some tools on github - kinesis scalaing utils
  * Amazon Kinesis Data Analytics
    * Easily process and analyze streaming data with standard SQL
    * Better at stateful processing versus Lambda which is more for stateless
    * Details
      * Create an application
        * Input
        * Code - SQL statements
          * can be aas complicated as needed
          * will autoscale to query complexity and incoming through put
        * Output
      * Powerful real-time applications
      * Easy to use, fully managed
      * Automatic elasticity
      * Easily to get started
      * if over 100k events per second you have to do some manual configuring/mapping to specific queries
  * Amazon Kinesis Data Firehose
    * Easily load streaming data into AWS
    * Details
      * For a particular use case
      * Getting data into persistence storage (like S3 or RDS or elastic search)
      * Buffering hints - how much and how long do you want to wait before it is written 
        * So you don't write a lot of tiny files  to S3 which would be innefficent you want to write nice clean records out
      * Data processing before it arrives, do ETL (using Lambdas) before going to redshift as an example
      * Set and forget approach
      * autoscales
      * there is a max threshold to control costs
      * can be combined with data streams for more complex work
      * includes some machine learning algorithms that can be access through SQL queries
* Amazon CloudWatch
  * What it does
    * Built in monitoring of AWS resoruces
    * Can build custom monitoring
    * Monitor and store logs
    * set alarms
    * view graphs and statisticss
    * Monitor and react to resource changes
  * new features
    * sub second monitoring
* CloudWatch logs subscritpion
  * deliver near real-time feed of log events to Kinesis or AWS Lambda
* Amazon Kinesis benefits and CWL subscription
  * Use Kinesis firehose to persist log data to another durable storage location: S3, Redshift, Elasticsearch services
    * Realtime grep logs concept
  * Use Kinesis Analytics to perform near real-time streaming analytics on the log data
    * Anomoaly detection
    * Aggregation
  * Use Kinesis Streams with a custom stream processing application to apply business logic to your log data
    * Alternate data destinations
    * data enrichment
* Monoriting application-specific metrics
  * use cloudwatch agent to send application logs to Cloudwatch logs
  * Analyze stream with Kinesis Analytics application
  * Persist raw log data to durablestorage with Kinesis Firehose
  * You can also just directly send information to Kensis
* Anomaly detection algorithm
  * Set alarm threshold based upon simple metrics - this sets off a lot of false positives/negatives
    * example even across regions of different sizes the same metrics don't work
  * RRCF - robust random cut forest
    * Takes datapoints and generates an anomoaly scoe
    * in N dimensions space it calculates the distance from any point to all the other points
    * Lots of configuration points
      * configure how big the trees in your forset are
       * smaller forcest & bigger trees or inverse
       * # Trees -> sub sample size
      * shingle size
      * time decay - how soon do you start discarding old information
    * Not trained on the fly 
      * set-up with a sample stream and play with what they are expecting to see
      * Simple to use, but not a replacement for a data scientist
    * New change - added attribution and directionality
     * attribution - says what part of the data caused the anomaly. Tells you what you should be looking at
     * directionality - telling whether the anomaly is trending up or trending down (getting worse or better?)
    * https://d0.awsstatic.com/whitepapers/kinesis-anomaly-detection-on-streaming-data.pdf
    * restarting the algorithm (redploying/chaning config) will cause it to produce 0's until the state builds up
    * Large number of keys and large volume of data can cause the algorthm to be a bit cludgy 
* Dealing with faults
  * For example malformed data
    * When set-up datasource sample data of source and detect the structure of the stream
    * If it doesn't validate against that schema then it will be sent to the error stream
    * For AWS lambda - the poison pills if its not right, the data will 

* How do I do deal with changing schema
  * API for schema discovery is independent from the application
  * Can call to update

* Exactly once processing
  * probably means that they just can't detect, impossible to guarantee
  * Effectively once is probably more realistic
  * Kensis provides at least once and try to be effectively once

* Monitoring network activity
  * use vpc flow logs to get visibility into application communication
  * Vpc flow log records contain network data taht can be analyzed
  * Lot of data, need to build something to take advantage of it
  * example
    * enrich source and destination with application infromation because IP address is not great
    * aggregate data by specific diemsions and persiste aggregated values
    * Is something wrog with the network?
      * What if service A can no longer talk to service D but everything else can tall to D?
        * Is this a bad deployment?
      * What if nothing in one zone can talk to another zone?
    * What are the application dependencies?
      * you have an expected design from the inital creation
      * You could find that there are dependencies you didn't expect (connection to other tools or services)

* 




