# DEV318 - Learn How Intuit Built a Frictionless Infrastructure Management System Using AWS CloudFormation

* Infrastructure continuous delivery
  * Infrastructure as code - where infrastructure is provisioned and managed using code and software development techniques
  * Worklfow - build test, and deploy your code everytime there is a code change, based on the release process modules you define, enabling you to rapidly 

* AWS cloud formation
  * Define infrastructure as code
  * Create templates of your infrastructure
  * version control/code review/update templates like code
  * CloudFormation provisions AWS resources basedon depenency needs
  * Integrates with development, CI/CD management tools
  * No additional charge to use

  * Author templates in JSON or YAML
  * Use change sets to preview your changes (see what would be done before it is done)
  * Enable cross-stack references with nesting & exports
  * Continious delvery workflows for stacks
  * Layer in protective controls

* Managing Infrastructure as Code
  * Some pain points
    * Control variability - same solution in 3 different places
    * productivity loss - management/find the right things
    * trust/confidence - how do you know it is good

* this all seems like classic CM problems and seems like failure of cloud formation as a tool

* Solutions
  * Requirements
    * reduce complexity of infrastructure
    * has to be frictionless
    * should be crafted to avoid random piles of scipts
    * must be accessible to our customers

  * Solution - process flow
    * Create
    * Develop - keeps the fork in sync per standard dev practives
    * Open pull request - automated build/test initiated
    * Review - community involvement, developer actively responds to review, changes generate new builds
    * complete - pull request is merged to master or closed, after merge artifacts can be released GA
  * Solution - build/test
    * Use GitHub
    * Use github organization
    * Using Lamdas as the inbetween handlers 
    * step functions to check through the process
    * use cfgnag to lint
  * Solution - step function
    * Check build state
    * Write badge to Gitbub
    * Write Github Status API
    * Send notifications
  
* Learnings
  * keep it simple
  * Stay flexible
  * people don't more work

