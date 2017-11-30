# CON308 - Mastering Kubernetes on AWS

* Kubernetes cluster setup choices
  * Minikube for development
  * Community - Kops
    * kubernetes-aws.io
  * Enterprise
    * elastic conainer service for kubernates (EKS)
    * core OS tectonic
    * red hat open shift
  * Custom
    * cloud formation
    * terraform
* Manage a kubernetes cluster: Kops
  * Community supported
  * Generates cloud formation or terra form scipts
  * github.com/kubernetes/kops
* Elastic container service for Kubernetes
  * Managed k8s control plane - highly available master and etcd
  * Bring worker nodes like ECS
  * Core tenents
    * enterprise grade workloads
    * 100% upstream K8's experience (no forking of the K8's side) - should work the same as minikube
    * Seemless, not not forced integration with other AWS services
  * hghly available URI so you don't have a per availability zone master and etcd
* Cluster setup at Zalando
  * Why kubernetes?
    * growth capacity
    * deployment strategy was custom and people had to be trained on it
    * initial approach was docker container on an ec2 instance
    * Gave high density
    * good abstraction
  * Muliple AWS accounts (one cluster per account)
    * currently ~50 kubernetes clusters
    * small clusters
      * limit impact of possible outages
  * Standard Linux OS, no AMI customization
  * Flannel to support > 50 nodes (because of limitation of routing tables in AWS)
  * nodes are updated in a rolling fashion (no reboots)
  * cluster operations
    * cluster registry in RDS
      * points to git repository for all configuration
    * cluster lifecycle manager
      * watches git repo
      * ensures it will apply to the cluster
* CI/CD of apps on Kubernetes
  * Look at Aws CodePipeline, AWS codeCommit, AWS codeBuild
  * In Build
    * Checkout
    * Build docker image
    * Push to ECR
    * deploy new application
    * update application
  * Process
    * AWS CodePipeline 
    * AWS code Commit (watches)
    * AWS codeBuild (build)
    * Lambda - does deploy
* AWS Identity and Access Management (IAM)
  * IAM enables secure acces to AWS services and resourcs
  * Two IAM roles created for K8s cluster
    * Master (has slightly more permissions)
    * Worker
  * Need to get this in as a first class working in k8s
    * current solution github.com/heptio/authenticator
      * enables Kubectl passes AWS idenity 
      * verifies aws identify
  * IAM role for Pods (at runtime)
    * popular solution github.com/jtblin/kube2iam
      * plugin installed as a DaemonSet in the pod
      * web service can get temporary credentials
      * web servce talks to security token service
      * then it uses that to talk to the metadata service
      * In the deployment annotation that is the IAM role and it is attached to the pod
      * Hazard is that the work node can assume any identity
    * Hashicorp valut
      * secret management and generate IAM crentials
    * Secure Protection Identify framework for everyone (SPIFFE)
      * in the future (under development)
      * securely passing secrets like IAM roles to pod/container
      * stanad that applies to container runtimes, in general
      * Enhance other CNCF projects
* IAM at Zalando
  * 2 systems (internal and AWS IAM)
    * use kube2IAm like described before
    * four-eyes principle for deployments (developers can choose the IAM role)
    * Internal IAM 
      * every microservice is OAuth protected
      * Authentication and authorization is based on a custom webhook
        * kubernetes native integration
        * slowly transitioning to RBAC (more flexible with cusomizing roles for the cluster)
    * Credential provider
      * Create creential and provides it
      * The API servce reads the crential and writes to the crential provier talks to the OAuth provider
      * The provider wries a secrete
* Visibility in cluster
  * Need visibility on the node, the container, the cluster, and the application and you want logs, metrics, events, alerts, and tracing
  * Want these to come together to create a holistic picture
  * Logs
    * Most basic way - kubectl logs - container writes to systemd and these get spit out
    * Elasticsearch (index), Fluentd(store), Kibana (visualize) - EFK - common stack
      * Fluentd is on each node (daemonset)
      * push the logs to the CloudWatch Logs
      * this is then pushed to Elasticsearch service
      * Kibana dashboard on top of that
  * Metrics
    * Nodes - node exporter (spits out info about the node)
    * Pod/Conainer - cAdvisor (OS) or Kube-state-metrics (build in)
    * Application - /metrics or JMX
    * Cluster-wide aggregator - prometheus (time series database) or heapster
    * Data Model - InfluxDB or Graphite
    * Altering - AlterManager or Kapacitor or custom solution
    * Visualizer - Grafana, Kibana, or Dashboard
  * Application tracing
    * Tools like AWS XRay help debug distributed application
    * end to end view of resquests as they travel through your application
* Visibility in cluster at Zalando
  * What works for them
    * Logging
      * Centeralized logging solution Scalyr
      * applications just log to Stdout
      * a daemonset streams logs to a centralized account
    * Monitoring
      * existing monitoring/alerting tool (custom built) - ZMON
      * Prometheus node exporter as DaemonSet to get mestrics
    * Application Monitoring - Alerts & Metrics
      * Default checks/alerts for the deployed applications
      * Metrics available in standard format from Ingress Controll
        * latency, error rate
        * Useful for service level reporting
    * Visualizer 
      * different dashboards for applications and for ops
* More things to configure
  * Configuring sane defauls (for example, LimitRange)
  * Knowing and understanding cluster limits
  * Simplifying user experience ( for example Ingress, externalDNS)
* aws-workship-for-kubernates - project in github in teh aws-samples space
  * has examples of how to deploy Kuberates on AWS
  * Different levels of how complicated it is
  
