# ABD310 - How FINRA Secures Its Big Data and Data Science Platform on AWS

* Data scientists are not technologies so need to be secure, seamless, but secure
* Herd - open source project they built which manages the metadata of the data

* risk of private data center is relatively comparable to the risks in the cloud
* greatest risks are indepenent of the cloud
* Key factors
  * compare risk of alternatives
  * evaluate risk in context
  * Cyber security is just one dimenstion of risk

Shared responsibility model
  * Security of the cloud 
    * customer due diligence through third-party risk management
    * AWS has skin in the game
  * Security in the cloud
    * Controls to suplement the cloud service provider (CSP)
  * Secuirty above the cloud
    * all the security controls that you are using/should be using
 
* Foundational security controls
  * AWS Provided
    * AWS best practices - follow them
  * Built/brought to cloud controls
* Acces management
  * Human
    * IAM SAML support - integrate with active directory. Map 1:1 IAM with active directory roles
      * Can only assume one role at a time
    * Multi factor authentication
  * Access Mgmt - Machine
    * Cloud pass (built by FINRA)
  * Robust Seperation of Duties
    * Role + application matrix explicit allow/deny - leads to almost 100,000 entitlements
    * For example don't give anyone direct delete access on an S3 bucket
  * Greenfield opprotunity to redefine roles and access fro those roles
  * Entitlemens
    * Scraping AWS docs
* Secrets management
  * Credstash - application credential vault
  * Minimize exposure of privleged credentials
  * Secrets stored in Dynamo DB; Encrypted with KMS
  * Resource/Object Owner creats/stores Secrets
  * Automation deploys secrets to Subject
    * IAM Role limits secrests access
 * Logging, monitoring, and alterting
  * Splunk, deep and breath 
  * Avoid Economic Denial of Service
    * Accounting what project is being used on what sources and capturing the billing
    * Managing costs by exposing them and having logic to map the value to places
* Network architecture
  * Uses up to 4 availability zones
  * cross-region replication when
    * geographic dispersion is needed
    * single reagion data durability is not
  * more robust availability control than private
* Encryption
  * seperate cloud provider risk here by using KMS seperate from 
  * KMS only where needed (not everywhere)
    * Data key throttling constraints make this impractical
    * Adds for operational complexity
* Security goverenence
  * Overall governency policies
  * Uniform process for creating and updating goverenency polcies
  * Body to steer policy
* Securing the services
  * Create new AMI's at least once a month
    * Start: Latest Amazon AMI
    * Harden - remove unneded packages
    * Extend - add common things
    * Snapshot - have a new AMI
  * Security Groups
    * Goals
      * Narrowly crafted microsegmentation
      * Policy of least privlenge
      * Seperation of duties
    * Chanllengs
      * Many groups to manage
    * Solution FINRA Portus
  * Strictly Limited access
    * Goal: No access to production
* Portus
  * seperate security policy setter and developer selections of the pre-determined policy
  * Only rules allowed the policy can be applied
* Gatekeeper (built by FINRA)
  * Get just in time acces to EC2 instances
  * Requests and get approval & create temporary accounts for access
* Securing S3
  * Encrypt at rest, using KMS where needed
  * At rest - HDFS encryption
* Securing the architecture
  * Data sanitization
    * One-way hash/tokenization - preserve ability to associate related records by the senstivie data element, search on tokenized values
    * Format-preseving encryption
    * preserves ability to associate related records, some limited ability to operate on data (search, sort, categorize)
    * Generalization, subsetting
    * De-identification
      * Be wary of re-identification strategies
  * Limit crential exposure
    * IAM role based access is Ideal
  * Make security easy
    * internal mirros of external resoruces
    * empower users, managers with utiliation/cost information, necessary entitlmenst to privde oversidte
* Striking the balance between security and productivity
  * Security
     * same security apply to all systems
     * Risks must be identified and mitigated
     * sufficent controls
  * Productivity
    * not get in the way
    * Want the easiest thing to tbe the best way
* Migration story
  * V0 - data in cloud, processing local
    * Needed more powerful machines so why 
  * Data science platform V1
    * no longer could own everything they wanted as their instances were in the cloud
    * Package access became a problem 
      * People were build libraries locally and pushing them to the cloud instances
    * too enerous for things to do
    * machines were only used when they needed to be (otherwise they were getting by what was on their laptop)
    * secure bu inflexible
    * couldn't reach on prem files from cloud
  * Universal data science platform V2
    * secure connection back inside/ fix connectivity issues
    * 20 fold increase (EC2 bill was huge)
    * Dashboard to monitor resource usage
    * Reports for historical usage
    * Managers can admister their team's instances
    * Users associated to groups
    * Completly self service, no technology administration

