#01 - ARC304 - From One to Many: Evolving VPC Design

* VPC Deisgn
  * Where
  * How big - CIDR block looking ahead is hard
    * Going as large as you can is the AWS recommendation
    * CIDR block fixed on VPC creation
  * CIDR in IPv4 Space and acannot change
  * Think of VPC as an extension of the network
    * Cannot overlap with IP space used for other apps

* IPv6 has been enabled recently
  * /56
  * Block provided by AWS instead of assigned
  * Not link local, globably available
    * How do you create a private subnet? - answered soon
* Resize the VPC capability has been added by adding a secondary CIDR block
  * Soft limit of 5 sub CIDR
  * Additional blocks become local route in main route table
    * Added autmatically, can't add or delete the route
  * Same restriction as far as overlapping CIDR blocks
  * Can add or remove the secondary CIDR blocks, but not the primary

* Subnet design
  * Subnet becomes a routing container, not a single application
  * E.g. not bound by traditional switching limitations
  * Mixed purpose subnets
    * Specific subnets for specific purposes
    * Create a public subnet (internet gateway bound to the route table)
    * Instances that run in a public subnet don't know they are in the public, the internet gateway and low level 
    * If you need internet connectivity (has NAT infrastructure) Have a hybrid subnet - can communicate online but not have online speak to it
    * Pure private if you don't need to communicate to the internet -

* IPv6 Space 
  * Routing policies must be explicitly stated in route table seperate from IPv4 (they won't transfer). They are significantly different
  * has a tech called internet egress only that can be used to restrict access to the "public" spcae (egw) Gate way that only allows outward but not inward connections

* Flat network, regardless of number of CIDR blocks
* Think of VPC as part of the network

* NAT scaling is a problem, so create NAT Gateway
  * Not a single infrastructure
  * Scales horizontally
  * HA, available in the Availability Zone it is created in
  * local to the availability zone
  * Can be zoned, and can evenly distribute traffic
  * How do you secure it?
    * Can't use security groups because of lack of control
    * Use NACLs to secure 
    * Can add policies to the Route table (don't specify the connection in areas to NAT gateway)

* Problems:
 * Too many apps to interact with?
 * What abou ingress control

* Elastic Load Balancing
  * Application load balancer  - layer 7 (HTTP/HTTPS & WS/WSS)
    * Do not have full control over the virtual instances
  * Network Load Balancer (NLB) - Layer 4 (TCP)
    * designed to handle spikes 
    * Elastic IP address - so easier configuration
    * Can zone traffic
    * Maintains source IP address infromation
    * Can access the virtual infrastructure associated with it
    * Can be modified by changing target group
    * Gives ability to fail over between availability zone
    * Only supports TCP, not UDP at this time
    * Target groups are not limited to Amazon EC2 & can be used with containers (especially useful for Docker or ECS instances in the network)
  * Classic Load balancer - Layer 4 (TCP, UDP, ...)
 
* Implications
  * The VPC is no longer the boudry for the load balancer
    * Internal/cloud difference can be hidden
    * Can be across availability zones  

* Egress Control
  * Use AWS Endpoint services
    * Not single instances
    * scales and is managed by AWS
  * Gateway enpoint and services endpoint

* Gatway VPC Endpoint
  * Legacy structure - Go through a NAT Gateway
  * Prefix list for explicit routing policies 
    * Overrides the default route
    * Just a name that maps to an Amazon address service
    * Don't have to manage what the actual IPS are
    * Cannot modify the route once it is added
 * Can control access s3/Dynamodb via IAM policy
 * Can create a bucket policy in S3/DynamoDB as well to marry the policies together 
 * Can use security group prefix list

 * Interface VPC Endpoint (new)
   * The public service becomes part of the VPC
   * No specific route policy
   * Specify the DNS host names to reach the public AWS service, handled as a local route
   * Specify public URL but it gets mapped by Amazon to the private address, that is then routed to the public service
   * Only supports TCP traffic
   * Secure it same way you secure EC2 instances
   * can restrict communication across availability zones so you don't get charged fro crossing them
   * why use
     * Use AWS service with private connections
     * specific to an Amazon region
     



