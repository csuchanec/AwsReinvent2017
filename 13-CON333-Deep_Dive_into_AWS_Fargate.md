# CON333 - Deep Dive into AWS Fargate

* Example showing a tic tac toe game - package will be on aws-labs on github

* Fargate constructs
  *  task definition
    * blue print for the task you want to run
    * immutable, versioned document
    * identified by family: version
      * family is a way to reference a set of versions
    * containers a list of up to 10 container definitions
    * all containers are co-located on the same host
    * each container definition has
       * a name
       * image url ( an amazon elastic container registry or public images)
       * cpu & memory specification
         * same as nromal, scpu and spu nits
       * task level resources
        * total cpu/memory across all containers
        * required fields
        * billing axis
       * container level resources
         * optional because of task level resources
  * cluster 
    * logical abstraction to organize tasks
    * infrastructure isloation
    * IAM permission boundary
  * task
    * instantiation of a task definition
    * FARGATE launch type
  * Service
    * maintain n running copies
    * integrated with ELB
    * Unhealty tasks can be replaced
 
* Container CPU sharing configuration
  * task CPU is the toal CPU available for all containers in the task definition
  * Container CPU is optional - by default all containers get equal share
  * specify container CPU to control relative sharing among containers - done as aratio

* Container Memory Sharing configuration
  * task memory is the total memory available fo all containers
  * container level memory settings are optional, by default all task memory is available to all containers
  * memory reservation is soft lower bound. Can kick when task memory is under contention
  * memory is a hard upper bound. container will not be allowed to grow beyond this value

* pay only for resources allocating, on a per second basis with a 1 mintute minimum

* Platform version
  * What is it?
    * it refers to a specific runtime environment around your task
     * combination of Kernel version & container runtime version
     * new version will be relased as the runtime environment evolves
  * why expose it
    * gives you explicit control oveer
      * migration to new platform versions
      * rollback if there are problems

* Networking
  * VPC integration
    * launch the fargate tasks into subnets
    * under the hood 
      * create an elastic network interface (ENI)
      * the eni is allocated a private IP from the subnet
      * the ENI is attached to the task
      * task now has a private IP from the subnet
      * you can assign public to IPs to tasks if you want them addressible by internet
    * you can configure security groups to control inbound and outbound traffic
    * spread your application across subnets in multip Availability zones for high redundency
  * VPC configuration
    * networkMode - awsvpc - enables ENI creation & attachment to task
    * On run-task  ( --network configuration ) provide the lists of subnets. If multiple it will try to try to spread across availability zones
  * Internet access
    * the task eni is used for all inbound & outbound network traffic for the task
    * it also used for 
      * image pull from ECR or public repository
      * pushing ot Amazon cloudwatch logs
    * both of these need to breachable via your task ENI
    * Two common modes of setup
      * private with no inbound internet traffic, but allows outbound internet access
        * Any traffic that you need to put out you need an internet gateway
        * create a public subnet seperate from the private subnet
        * create a route in the rout table with the public subnet
        * inside the public subet add a NAT gateway public EIP that can connect out
        * allow private vpc to connect to the NAT gateway and configure the route table to forward there
        * security group will need to allow the access
      * public task with both inbound and outbound internet access
        * need a public subnet, the internet gateway
        * launch the task into the public subnet
        * task needs to give a public IP address
        * In cli add as part of --network-configuration - assignPiublicIp=ENABLED
        * routes need to allow inbound traffic as well
        * Security group needs to allow the 
        * discover th epublic IP by describing the ENI attached to the task
  * ELB Setup
    * ELB integration supported on services
    * ALB & NLB supported
    * ALB requires you pick at least two subnets in two different AZs
    * ensure that the ALB subnet AZs are a superset of your task subnet AZ
    * Select ALB target type: IP (not Instance)
    * ELB Configuration
      * add portMappings
        * add containerPort so what should be registerd
      * in create-service - with --load-balancers parameter

    * setup is configure to the private task setup
      * want to keep the task private
      * replace gateway with ELB/ALB
      * ALB has security group to allow inbound traffic
      * Task security group to allow inbound traffic from the ALB security group

* Permissions
  * Permission tiers
    * Cluster permissions 
      * who can launch, stop, and describe in the cluster
    * Application permissions
      * allows your application containers to access AWS resources securely
      * how to get credentials down to the task?
        * using an IAM Task Role
        * IAM Role that requisite permissions that the application needs
        * establish a trust relationship with ecs on that role. This lets tfargate assume the role and wiere the credentials down
        * add the role arn to the task definition
        * credentials are temporary and rotated periodically
    * Housekeeping permissions
      * allows fargate to perform housekeeping activies around the task
        * ECR image pull
        * CloudWatch Logs pushing
        * ENI creation
        * Register/deregister targets into ELB
      * Execution role
        * what to do
          * ECR image pull
          * Push to cloud watch logs
        * create IAM Role and add permissions to read/push
        * establish trust relations with ecs
        * add the execution role arn in the task definition
      * ECS Service Linked Role
        * what it does
          * ENI management
          * ELB Target Registration/deregistration
        * IAM role that is linked directly to an AWS service
        * it has a predfined policy that is immutable
        * it has a trust relationshio
        * automatically created
        * don't have to explictly create it, it is already there

* Logginc & debugging
  * native integration with cloud watch logs
    * Use the aws log driver to send stdout from your application to CloudWatch logs
    * create a log group in cloudwatch
    * configure the log driver in the definition 
  * CloudWatch metrics integration - viewable in the ECS management console
    * have you over or under provisioned?
  * debugging tips
    * What if the task is not running?
      * inspect container stopped reason in the Task Detail page or via DescribeTasks API
    * Service not scaling as expected
      * in service page go to the Events tab (like an activity log) or Descirbe Services API
      * inspect the messages, what tasks were started and at what time
* Storage
  * ephemeral storage backed by Amazon EBS
  * layer storage space - 10 GB per task
  * volume for scratch space
    * 4 GB per task
    * mount points specified in Task Definition
    * can be shared across multiple containers

