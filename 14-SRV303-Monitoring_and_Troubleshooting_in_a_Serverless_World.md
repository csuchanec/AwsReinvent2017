# SRV303 - Monitoring and Troubleshooting in a Serverless World

* What is serverless
  * Building application without thinking about servers
  * Computing has moved from data cetners -> cloud
    * you still have to think about provisioning and scalaing, availability, fault tolerence
  * customer asks
    * no provisioning - run code without scal
  * AWS Lambda - serveless computing - without thinking where it is running or scaling. don't pay for idle time

* run code in response to events
  * triggers from event source
  * Function
  * services
* moves to a microservice architecture

* Serverless apps in production
  * monitroing performance and execution across microservices brings new challenges
    * resources typically only exist during execution
    * U

* General troublshooting tips
  * test functions locally
  * Use SAM Local to test - attach debugger

* Monitoring in production
  * collect metrics and logs in real-time
  * publishing metrics and logs shoud not impact application performance
  * Troubleshoot service issues - correlate failures or issues across different services
* Logging
  * Lambda has a built in agent to send logs to CloudWatch
    * default logging to CloudWatch: START, END, Report for each request
    * emit your own log entries using console.log
    * No latency impact
* Metrics
  * Lambda natively monitors & reports metrics through CLoudWatch Metrics
  * built in metrics for lambdas
  * new metric for per function concurrency
  * Create dashboards and set alarms
  * can put custom metrics
  * What if you want more?
    * emit logs in an appropriate format
    * add log filter to create new metrics in CloudWatch
    * or use 3rd party tools for log aggeregation and visualization

* Create custom metrics and logs
  * emit your own logs in custom formats with console.log(specifc)
  * process this with a lambda
  * publish downstream
  * visualize/search as need be

* Troubleshooting service issues
  * As the application gets more complicated it gets more and more difficult to troubleshoot with just logs
  * hard to correlate across all different services
  * want to be able to look at an application in a service graph sort of pattern

* AWS X-Ray
  * Makes it easy to 
    * identify performance bottlenecks and errors
     * pinpoint issues to specific services
     * idnentify impact of issues on users of the application
     * visualize the service call graph of your application
* Lambda with AWS X-Ray
  * X-Ray agent is natively built into Lambda
  * Enable by turning tracing 'active' for your function
  * Capture metadata for calls to othe AWS Services
  * happens with low latency

  * It displayes the data in a visual format
    * three types of nodes
      * lambda service
      * lambda function

* X-Ray concepts
 * Trace - end to end data relatied to a single request across services
 * Segments - built in level of split up work
 * sub-segments - custom add split up work into smaller parts
 * annotations - tagging
   * can add it to segment/subsegment that enables a different categorization
 * metadata
 * errors

* X-ray 
 * provides an API to send, filter, and tretrive
 * raw trace data is available
 * you can bild your own data analsysis applications on top of it

* Case study
  * Trying out X-Ray
    * able to instrument the applications with little effor
    * able to see a sampling of user traffic
    * able to quickly find and address performance concerns
    * Increased granularity with annotations
    * Notes:
      * you won't get all errors sampled unless you add - forceSamplingOfCurrentSegment()
      * Can't annotate root segment so you need to add a "root" sub-segment with annotation and metadata
      * inferred segment based off of sub-segments with services that down issue thier own segments

* Recap
  * Servless apps have built in logging & monitoring capabilities
  * easy to extend with CloudWatch metrics, Amazon ES, and third party tools
  * AWS X-RAy is powerful tool to visualize and troubleshoot issues