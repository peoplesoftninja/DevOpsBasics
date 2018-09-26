
- [Topics I need to learn](#topics-i-need-to-learn)
    - [Change Configuration (Chef, Puppet, Salt Stack, **Ansible**)](#change-configuration-chef-puppet-salt-stack-ansible)
    - [Cloud Native Infrastructure, O'Reilly book on it (service discovery, reverse proxy to service mesh, deployment patterns, web traffic), beyond 12-Factor](#cloud-native-infrastructure-oreilly-book-on-it-service-discovery-reverse-proxy-to-service-mesh-deployment-patterns-web-traffic-beyond-12-factor)
    - [Basic systems engineering and network engineering, database and/or no-sql exposure](#basic-systems-engineering-and-network-engineering-database-andor-no-sql-exposure)
    - [CICD continuous integration-delivery pipeline (**Jenkins**, **Gitlab**)](#cicd-continuous-integration-delivery-pipeline-jenkins-gitlab)
    - [Infrastructure As Code(**Terraform**, CloudFormation,](#infrastructure-as-codeterraform-cloudformation)
    - [Ideally two cloud providers, minimal one (AWS, GCP, Azure), and bonus for hybrid-cloud (OpenStack, vSphere, Cobbler, FAI, etc)](#ideally-two-cloud-providers-minimal-one-aws-gcp-azure-and-bonus-for-hybrid-cloud-openstack-vsphere-cobbler-fai-etc)
    - [optional - local development tool-chain (vagrant, docker-compose, test-kitchen)](#optional---local-development-tool-chain-vagrant-docker-compose-test-kitchen)
- [Source Control Management](#source-control-management)
    - [Git](#git)
- [Build Automation](#build-automation)
    - [Gradle Install](#gradle-install)
    - [Gradle Wrapper](#gradle-wrapper)
    - [Gradle Basics](#gradle-basics)
    - [Automation Testing](#automation-testing)
    - [Excercise](#excercise)
- [Continous Integration](#continous-integration)
    - [Jenkins Install](#jenkins-install)
    - [Setting up Jenkins Project](#setting-up-jenkins-project)
- [Continous Delivery](#continous-delivery)
    - [Jenkins Pipeline](#jenkins-pipeline)
- [Containers](#containers)
- [Orchestration](#orchestration)
- [Monitoring](#monitoring)
- [Self-Healing](#self-healing)
- [Auto Scaling](#auto-scaling)
- [Canary Testing](#canary-testing)
- [Fully Automated Deployment](#fully-automated-deployment)
# Topics I need to learn

## Change Configuration (Chef, Puppet, Salt Stack, **Ansible**)

## Cloud Native Infrastructure, O'Reilly book on it (service discovery, reverse proxy to service mesh, deployment patterns, web traffic), beyond 12-Factor
## Basic systems engineering and network engineering, database and/or no-sql exposure
## CICD continuous integration-delivery pipeline (**Jenkins**, **Gitlab**)
## Infrastructure As Code(**Terraform**, CloudFormation, 
## Ideally two cloud providers, minimal one (AWS, GCP, Azure), and bonus for hybrid-cloud (OpenStack, vSphere, Cobbler, FAI, etc)
## optional - local development tool-chain (vagrant, docker-compose, test-kitchen)

Basically, learn how to build, test, deploy a cloud application and supporting infrastructure, and run the testing and delivery through a CI tool. All the concepts come into play.

# Source Control Management

* Huge part of DevOps Workflow, all changes made by dev are tracked here
* Devs will use SCM to track their changes separately and merge it together
* SCM needs to notify the CI server when the code needs to be build

## Git

* Cloning: Pulling out a copy of source code from remote repo `git clone url.git` or `git pull url.git`
* Adding: When a code is changed we add it to staging before we commit it `git add *`
* Committing: We add the code to Git repo local `git commit -m "comment"`
* Pushing: We push the code to remote server from our local repo `git push origin master`
* Status: Gives all information about current repo `git status`
* Branching: Usually a copy of master is done for a particular feature and all push is done to that and then later merged
* Merging: 
* Pull Requests: Sending a request before merging and it gets reviewed before an actual merge is performed

Learn more [here](https://github.com/peoplesoftninja/GitQuickTools)

# Build Automation

* Using Build Tool [Gradle](https://gradle.org/)
* Build Automation is the automation of tasks needed in order to process and prepare source code for deployment in Production, this includes
    * Compiling (In Python use make)
    * Dependency management (pip, setuptools)
    * Executing Automated Tasks (pytest)
    * Packaging the app for Deployment (In python bdist_wheel, setuptools)

## Gradle Install

* [Instructions](https://gradle.org/install/)

```s
$ cd ~/
$ wget -O ~/gradle-4.7-bin.zip https://services.gradle.org/distributions/gradle-4.7-bin.zip # Search for gradle distribution
$ sudo yum -y install unzip java-1.8.0-openjdk # downloading dependencies
$ sudo mkdir /opt/gradle
$sudo unzip -d /opt/gradle/ ~/gradle-4.7-bin.zip # -d is to give the directory location to unzip the contents
$ sudo vi /etc/profile.d/gradle.sh
```
* Add the following command to the file `export PATH=$PATH:/opt/gradle/gradle-4.7/bin` and then give proper permissions `sudo chmod 755 /etc/profile.d/gradle.sh`
* After you exit out and log back in you can check the version by `gradle --version`

## Gradle Wrapper

* This is basically a script which will make sure that Gradle and other dependencies are installed, so you just give this script with your source code and the just by running everything gets installed and build is started to build
* From the Gradle site "The Wrapper is a script that invokes a declared version of Gradle, downloading it beforehand if necessary"
* Useful because
    * removes the need to install Gradle before build
    * Ensures project is being run on the same version of gradle in which it was tested
    * Lets you build multiple project with different gradle version on system
    * Anyone can run build quickly they only need Java

```s
cd ~/
$ mkdir my-project
$ cd my-project
$ gradle wrapper # install wrapper
$ ls -la
total 16
drwxrwxr-x.  4 user user   65 Sep 20 14:38 .
drwx------. 14 user user 4096 Sep 20 14:38 ..
drwxrwxr-x.  3 user user   20 Sep 20 14:38 gradle
drwxrwxr-x.  5 user user   54 Sep 20 14:39 .gradle # add this to .gitignore
-rwxrwxr-x.  1 user user 5296 Sep 20 14:38 gradlew # for linux
-rw-rw-r--.  1 user user 2260 Sep 20 14:38 gradlew.bat # for windows
$ ./gradlew # will install gradle and build if gradle not installed
$ ./gradlew build # will build
```

## Gradle Basics

* use `gradle init` to initalize a new project. This also sets up the gradle wrapper for you
* build.gradle is located in the root directory
  
* Running a Gradle Build: Gradle build consist of a set of tasks that you call from command line
* `./gradlew sayHello sayBye` will run a task name sayHello and a task name sayBye

* The `build.gradle` file controls what tasks are available for your project
* A task's dependencies will always run before that task and is defined in build.gradle like `taskA.dependsOn taskB`

* Plugins: Gradle has a lot of plugins, available on `https://plugins.gradle.org/`

```s
cicd-pipeline-train-schedule-git[master] $ ls -la
total 52
drwxrwxr-x. 8 user user  4096 Sep 18 14:06 .
drwxrwxr-x. 5 user user    66 Sep 18 14:06 ..
-rw-rw-r--. 1 user user  1079 Sep 18 14:06 app.js
drwxrwxr-x. 2 user user    16 Sep 18 14:06 bin
drwxrwxr-x. 2 user user    24 Sep 18 14:06 data
drwxrwxr-x. 8 user user  4096 Sep 20 23:00 .git
-rw-rw-r--. 1 user user    13 Sep 18 14:06 .gitignore
-rw-rw-r--. 1 user user   343 Sep 18 14:06 package.json
-rw-rw-r--. 1 user user 27225 Sep 18 14:06 package-lock.json
drwxrwxr-x. 4 user user    42 Sep 18 14:06 public
-rw-rw-r--. 1 user user   582 Sep 18 14:06 README.md
drwxrwxr-x. 2 user user    37 Sep 18 14:06 routes
drwxrwxr-x. 2 user user    58 Sep 18 14:12 views

# running gradle init generates below files
cicd-pipeline-train-schedule-git[master] $ gradle init
Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 7s
2 actionable tasks: 2 executed
cicd-pipeline-train-schedule-git[master ?] $ ls -la
total 72
drwxrwxr-x. 10 user user  4096 Sep 20 23:02 .
drwxrwxr-x.  5 user user    66 Sep 18 14:06 ..
-rw-rw-r--.  1 user user  1079 Sep 18 14:06 app.js
drwxrwxr-x.  2 user user    16 Sep 18 14:06 bin
-rw-rw-r--.  1 user user   201 Sep 20 23:02 build.gradle
drwxrwxr-x.  2 user user    24 Sep 18 14:06 data
drwxrwxr-x.  8 user user  4096 Sep 20 23:02 .git
-rw-rw-r--.  1 user user    13 Sep 18 14:06 .gitignore
drwxrwxr-x.  3 user user    20 Sep 20 23:02 gradle
drwxrwxr-x.  4 user user    42 Sep 20 23:02 .gradle
-rwxrwxr-x.  1 user user  5296 Sep 20 23:02 gradlew
-rw-rw-r--.  1 user user  2260 Sep 20 23:02 gradlew.bat
-rw-rw-r--.  1 user user   343 Sep 18 14:06 package.json
-rw-rw-r--.  1 user user 27225 Sep 18 14:06 package-lock.json
drwxrwxr-x.  4 user user    42 Sep 18 14:06 public
-rw-rw-r--.  1 user user   582 Sep 18 14:06 README.md
drwxrwxr-x.  2 user user    37 Sep 18 14:06 routes
-rw-rw-r--.  1 user user   380 Sep 20 23:02 settings.gradle
drwxrwxr-x.  2 user user    58 Sep 18 14:12 views

```

build.gradle
```js
plugins {
  id 'com.moowork.node' version '1.2.0'
}

task sayHello  {
  println 'Hello, World!'
}

task anotherTask  {
  println 'This is another task'
}
sayHello.dependsOn anotherTask
```

* now if we run `./gradlew sayHello`, it will always run anotherTask first
* If we simply run ./gradlew build, it will make sure anotherTask runs before sayHello
* plugin can be used by `./gradlew nodeSetup`. See documents for more

## Automation Testing

* Automation tests are usually run as part of build process and executed using build tools like Gradle

* Types of Automated Testing
    * Unit Tests - Focused on testing small pieces of code in isolation, usually a single method or a function
    * Integraton tests - test larger portions of an application that are integrated with each other
    * Smoke Tests / Sanity Tests - High level integration tests that verify basic, large-scale things like whether or not the aplication runs, whether applicatoin endpoints return http 500 errors, etc.

* Automated Testing on Gradle is done by following
    * task build
    * task test
    * build.dependsOn test
* When we run `./gradlew build` test will run too

## Excercise

Instructions & Tasks
In this scenario, developers have created the first prototype of the train-schedule app. They need you to create a Gradle build for it. The developers need this build to execute some automated tests that they have written. If the automated tests pass, the build needs to produce a packaged archive that includes the application code and its dependencies. The archive should be located in the project folder as dist/trainSchedule.zip.

In short, this automation needs to:

1. Have a build task that can be called to execute the entire build process
2. Execute some automated tests (the build should fail if any of the tests fail)
3. Package the code and its dependencies into a zip archive called dist/trainSchedule.zip

In order to accomplish this, you will need to:

1. Configure git for ssh authentication with GitHub.com
2. Create a personal fork of the sample repository https://github.com/linuxacademy/cicd-pipeline-train-schedule-gradle
3. Clone your personal fork from GitHub
4. Initialize the project with gradle
5. Create a gradle build task
6. Include the `com.moowork.node` plugin in the gradle project
7. Execute automated tests as part of the build with the npm_test task
8. Create a task to generate a zip archive (dist/trainSchedule.zip) of the files that need to be depoyed to production
9. Make sure task dependencies are set up so tasks execute in the correct order
10. Commit and push these changes to your GitHub fork
Some useful links:

Gradle wrapper documentation: https://docs.gradle.org/current/userguide/gradle_wrapper.html

* Step 1: Setup git

```s
$ git config --global user.name "name"
$ git config --global user.email "email"
$ ssh-keygen -t rsa -b 4096 # create ssh key
$ cat /home/user/.ssh/id_rsa.pub # copy key and paste it in github account
$ git clone git@github.com:peoplesoftninja/cicd-pipeline-train-schedule-gradle.git
# use clone with ssh, if clone with https is used then you will need user/password ssh key won't work
```
* Step 2: Install gradle `./gradlew build` This creates build.gradle and other files
* Step 3: Edit `build.gradle`
  

build.gradle
```javascript
plugins {
  //inlude the nodeJS plugin to execute nodejs and npm tasks
  id "com.moowork.node" version "1.2.0"
}

node {
	download = true
}

//declare a build task
task build

//declare a task to create a zip of the app
task zip(type: Zip) {
	from ('.') {
		include "*"
		include "bin/**"
		include "data/**"
		include "node_modules/**"
		include "public/**"
		include "routes/**"
		include "views/**"
	}
	destinationDir(file("dist"))
	baseName "trainSchedule"
}

//declare task dependencies
build.dependsOn zip
zip.dependsOn npm_build
npm_build.dependsOn npm_test
npm_test.dependsOn npmInstall
npm_build.dependsOn npmInstall
```
* Step 4: Run `./gradlew build` this will create the dist folder which will have trainSchedule.zip
* Step 5: Git add,commit,push

* If we clone a new project which has ./gradlew file then we dont' have to install gradle, we can just use the above command and it will take care of itself. But it is important to have java installed. 
  
# Continous Integration

* CI is all about the practice of frequently merging code changes
* But, building the code manually will be a lot of work if we do frequent merge. So we automate it
* So CI is all about the practice of merging code but to fascilate it we automate Build process
* A CI server executes a build that automatically prepares and compiles the code and run automated tests
* usually, this is trigger by code changes at the source control
* If something goes wrong, the build fails and the team gets immediate feedback, via server sending email or slack message
* We use Jenkins as an Automation tool. `The leading open source automation server, Jenkins provides hundreds of plugins to support building, deploying and automating any project`

## Jenkins Install

* CentOS
```s
# removing java and installing compatible version of java
sudo yum -y remove java
sudo yum -y install java-1.8.0-openjdk
# setting up config files in yum to install jenkins from it
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum -y install jenkins-2.121.1
# to make sure jenkins start whenever the server is rebooted
sudo systemctl enable jenkins
# to start the jenkins
sudo systemctl start jenkins
# to see the logs
sudo tail -f /var/log/jenkins/jenkins.log
```
* Once you install you can access the UI from the publicIP:8080 in the browser. You will need to enter the password from the given log file location when you access it for the first time

## Setting up Jenkins Project

* Jenkins `Project` or `Job` is basically the configuration which controls what automation needs to be executed
* Simplest implementatoin is `freestyle project`
* New Item-->Name-->Free Style--Description--SourCode(Git) Give URL, no need for authentication-->Build Add Build Step Select Gradle, Gradle Trigger, Task Build--> Post-build actions zip file to archive `dist/bla.zip`
* You can set up git hooks, this will let jenkins build whenever you push code to git
* `webhook` is event notification made from one app to another over http

* Configuring Webhooks in Jenkins
    * 1st step is to connect jenkins to your github account
    * Create an access token in Github that has permissions to read and create webhooks
        * settings-developersettings-personalaccesstoken-generatenewtoken(adminrepohook)
    * Add a github server in Jenkins for Github.com
    * Create a Jenkins Credential with the token and configure the Github server config to use it
    * Check `Manage Hooks` for the Github server config
        * ManageJenkins-JenKinsLocation(VerifyURL)--Github-AddGithubServer-AddCreds--SecreText(Kind)--secret(above key)--giveID-giveDesc--clickAdd--credentials(selectKeyFromDropDown)--check ManagedHooks box--save
    * Test if the connection works
    * In the project config, under `Build Triggers` select `Github hook trigggers for GITScm Polling` this allows it to read webhooks



# Continous Delivery

## Jenkins Pipeline

* Pipeline is a collection of various Jenkins Plugins that support CD in Jenkins
* It takes source code through a pipeline from source code creation to production deployment
* [Documentation](https://jenkins.io/doc/book/pipeline/)
* Pipeline adheres to the best practice of IaaC (Infrastructure as a code), which means the entire pipleine is treated as a code and kept in source control as a file along with all the other code. 
* To create a pipeline, create a file called jenkinsfile and add it to source repo.
* instead of freestyle project while creating project you will use pipeline
* Jenkins support scripted and declarative language for pipelinefile


# Containers

2-3

# Orchestration

1-2

# Monitoring

1-2

# Self-Healing
# Auto Scaling

# Canary Testing

2-3

# Fully Automated Deployment

1-2
