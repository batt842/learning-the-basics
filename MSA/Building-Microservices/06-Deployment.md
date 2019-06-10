# 6 Deployment
* Monolithic application : easy to deploy
* Microservices : of course complicated

## 1 Brief Introduction to Continuous Integration
Core goal : to keep everyone in sync to each other by integrating newly checked-in code with existing code.

Artifact : built(only once for test and deployment), tested, deployed or placed in a repository

### 1.1 Are You Really Doing It?
The use of a CI tool is not the practice of CI.

Three Questions by Jez Humble
- Do you check in to mainline once per day.
- Do you have a suite of tests to validate your changes?
- When the build is broken, is it the #1 priority of the team to fix it?

## 2 Mapping Continuous Integration into Microservices
Single repository -> monolithic build -> multiple services
- pros: simple, works perfectly with lock-stop releases; makes sense for short period of time
- cons: heavy build & test = time-consuming; hard to find which service to deploy (when falling back, some organization just deploys them all); build failure blocks all other changes
Single source tree(single repository but separate parts) -> separate build -> multiple services
- pros: easy to map the builds certain parts of the source tree, check-in/check-out process is simple
- cons: easy to check code for multiple services at once (bad habit); easy to couple services
Multiple repositories -> multiple builds -> multiple services
- clear team owner-ship
- difficult to make changes across repositories but better than downside of the monolithic source control and build process

## 3 Build Pipelines and Continuous Delivery
Build pipeline
- Breaks down tests into stages; one stage for the faster tests, one for the slower tests
- Gives us a way of tracking the progress of our software and giving us insight into the quality of our software
Continuous Delivery
- the approach whereby we get content feedbacks on the production readiness of each and every check-in, and furthermore treat each and every check-in as a release candidate
- We need a model and clearance for release -> improves visibility of the quality of software; reduce the time between releases
- One pipeline per service -> multiple pipelines
Standard build pipeline (example)
- Compile & fast tests
- Slow tests
- UAT (manually tested and marked as success)
- Performance testing
- Production

### 3.1 And the Inevitable Exception
There are times having all services in a single build to reduce the cost of cross-service changes may make sense.
If you are unable to get stability in service boundaries in order to properly separate them,
- merge them back into a more monolithic service
- give yourself time to get to grips with the domain


## 4 Platform-Specific Artifacts
Ruby gem, Java JAR/WAR, Python egg, …

Artifacts may be executable or may need other runners like Apache or Nginx, …

It’s necessary to install and configure other software which our artifacts need.

What if we have a mix of technologies which bother us when testing and deploying.

Automated configuration management tools (usually support multiple technologies)

Puppet, Chef, and Ansible

## 5 Operating System Artifacts
OS-specific(=native) artifacts - RPM for RedHat or CentOS, deb for Ubuntu, MSI for Windows, …

We don’t care what underlying technology is : OS tools install/uninstall the package, get information about the package. Even on Linux, OS tools will automatically install dependencies.

It’s difficult to create : FPM package manager tool is good. Windows package system isn’t good. NuGet/Chocolatey NuGet is helpful.

How about multiple operating systems? - Unify or Reduce

## 6 Custom Images
Downsides of automated configuration management tools 
- Longer and longer amounts of time needed for provisioning of these dependencies
- or shutting down and spinning up new instances
- So, the declarative nature of these configuration management tools may be of limited use.
- Even, it can lead to increased downtime (blue/green deployment can help mitigate this)
One approach : Creating a virtual machine image that bakes in some of the common dependencies
- A significant time saving for installing dependencies
- But- building a image can take a long time, some of the resulting images can be large
Packer
- in case of dealing with various types of images; VMWare, AMI, Vagrant image, or Rackspace image…
- Packer creates images for different platforms from the same configuration
- like this… Vagrant image for local development and test, AMI for production AWS environment

### 6.1 Images as Artifacts
We can bake our service into an image and adopt the model fo our service artifact being an image.

-> a nice way of abstracting out the differences in the technology stacks. (just as with OS-specific packages)

### 6.2 Immutable Servers
Configuration drift problem - the code (or environment configuration) in source control no longer reflects the configuration of the running host

No changes should not be made to a running server. Any change, no matter how small, hs to go through a build pipeline in order to create a new machine

-> Straightforward deployments, easer-to-reason-about environments.

## 7 Environments
Same service but different environments(test, slower test, UAT, performance test, production, …) - can introduce a few problems

Your environment should be more production-like to catch any problems associated with these environmental differences sooner. But… can be expensive.

Using a production-like environment can slow down feedback loops.

So, we have to make compromises : have to consider money and speed, the balance is not static.

## 8 Service Configuration
Configuration changes in different environments should be minimum, or we will find problems.

Solutions
- One artifact per environment : it violates the concept of CD + what if Artifact-Test passes tests but Artifact-Prod ends up failing in the production environment? + more time + we need to know at build time which environment or about sensitive configuration data
- Single artifact + separate configuration- properties files, different parameters, …
- Using a dedicated system for providing configuration

## 9 Service-to-Host Mapping
Let’s use the term host rather than machine or box.

### 9.1 Multiple Services Per Host
From a host management point of view, it’s simpler + less expensive

Difficult monitoring, complex deployment, bad for autonomy of teams (+ interference)

### 9.2 Application Containers
(A special case of the multiple services per host.)

Good for improving manageability, reducing overhead

But..
- constraint on technology choice (+automation and management following a certain technology)
- The monitoring capability isn’t sufficient, spin-up times are slow
- Doing lifecycle management is difficult, analyzing resource use and threads is complex, and not free in a term of finance or resource overhead, …
It would be recommended to look at self-contained deployable microservices as artifacts.

### 9.3 Single Service Per Host
Preferable.

Simple monitoring/remediation, reduced single points of failure, easy to scale, good for security, …

We have opened up the potential to use alternative deployment techniques such as image-based deployments, the immutable service pattern….

If PaaS not available, this model is good to reduce a system’s overall complexity. Easy to reason about and can help reduce complexity.

Maybe too many servers…

### 9.4 Platform as a Service
Promising.

It runs an artifact.

It does so many things for users. We don’t have to have much control.

## 10 Automation
Very important!

Small hosts or even many hosts are easy to manage. Developers can remain productive.

Ideally, developers should have access to exactly the same tool chain as is used for deployment of our production services so as to ensure that we can spot problems early on.

### 10.1 Two Case Studies on the Powerful Automation
The cases of REA and Gilt. The automation tools boosted their speed.

## 11 From Physical to Virtual
### 11.1 Traditional Virtualization
Type 2 virtualization, hypervisor : Maps resources like CPU and memory from the virtual host to the physical host. Acts as a control layer, allowing us to manipulate the virtual machines themselves.

Resource-consuming

### 11.2 Vagrant
A deployment system good for dev and test rather than production. (running with standard virtualization systems such as VirtualBox, …)

This makes it easier for you to create production-like environments on you local machine.

You may not be able to bring up your entire system on your local machine.

### 11.3 Linux Containers
The most popular : LXC

Each container is effectively a subtree of the overall system process tree.

No hypervisor.

OS and containers share the same kernel. : The host OS could be Ubuntu, and the container’s OS could be CentOS at the same time.

Much faster than provision than VM. More of them can run on the same hardware than VM. Isolated. Much more cost effective.
Run on VM OS too. (of course)

Things to consider
- You need some way to route the outside world. (IP forwarding?)
- Containers are not completely sealed from each other.

### 11.4 Docker
Super Promising (Now, it rules.)

Container provisioning, network problems, and store/version Docker application.

It’s too recent technology to summarize with the book released 2015 Feb…

## 12 A Deployment Interface
We need a uniform interface to deploy from local, dev, test, and even to production deployments.

The best so far is - a single, parametrizable command-line call which can be triggered by scripts, CI tool, or hand.
~~~
eg. $ deploy artifact=[ENTITY NAME] environment=[ENV] version=[VER]
~~~

### 12.1 Environment Definition
~~~
IaC : Infrastructure as Code
YAML e.g. for AWS)
development:
  nodes:
  - ami_id: ami-XXXX
    size: t1.micro
    credentials_name: eu-west-ssh
    services: [my-service]
    region: my-region
~~~
~~~
production:
  nodes:
  - ami_id: ami-XXXX
    size: m3.xlarge
    credentials_name: prod-credentials
    services: [my-service]
    number: 5
~~~

## 13 Summary
Nothing new…
