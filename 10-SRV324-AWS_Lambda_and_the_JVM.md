# SRV324 AWS Lambda and the JVM

* What is servless?
* github.com/symphoniacloud/reinvent-2017
* blog.symphonia.io
* symphonia.io - severless consulting

* Choosing the JVM runtime
  * Familuarity
  * asynchronous & high throughput - matches well for JVM
    * keeping lamda functions warm is a solid use case (cold start is costly)
    * Pay at the begining but steady state the JVM runtime performs better
  * Cold starts
    * steps
      * instantiate a container
      * download, unpack your code + libraries
      * initalize Java
      * initialize your classes
      * Finally, deserialize and handle the incoming event
    * when happens
      * instantiated for the first time
      * scale out
      * new deploy/new code
      * config change
      * timeout (cools, and tear down)
    * how to detect
      * logging in initialization
      * X-Ray
      * CloudWatch logs - log group has log streams which reperesents a container, which represents a cold start (this is a leaky abstraction - don't count on it existing)
* Performance tips
  * cull dependencies
    * Mavin dependency tree helps
    * SBT dependency stats (will tell you all the dependencies and their size)
  * watch for other libraries pulling in transitive dependences (like the entire AWS SDK)
    * the Kenesis client library (KCL) - which needed to handle Kenesis records - will put the Jar at 12 MB and 10K classes
  * single responsibility principle enables horizontal scaling
  * take advantage of local caching (memory or disk)
* Other tips
  * trim (or don't include) unnecssary fields from event classes
  * vendor or copy/paste third-party even classes
  * Serialization behavior is not static - AWS change/fix the runtime
  * Use 'requestId'-aware lambda
    * Use it for logging (you have to use the log4j)
    * Can be used to dedup the invocation
* Structuing projects (thinking in Maven terms)
  * submodule per lambda
  * SAM (like cloud formation) at the root
  * Use the BOM for AWS SDK dependencies
  * Reporducible builds are imporant to avoid unnecssary deploys
    * don't want to make deploys to lambdas that haven't actually changed because doing so will cause them to cold start
  * pre-warm Maven cache
    * in your build your changes will build slower
    * code build has this built in
* essential question is does the SLA support the overhead of cold starts
* Zappa project as someone who has done work to optimize the lambda 
* io.github.zlika - reproducible maven build plugin

