- [Topics I need to learn](#topics-i-need-to-learn)
- [Building Packaging and Deployment](#building-packaging-and-deployment)
- [Source Control Management](#source-control-management)
    - [Git](#git)
- [Build Automation](#build-automation)
    - [Gradle Install](#gradle-install)
    - [Gradle Wrapper](#gradle-wrapper)
    - [Gradle Basics](#gradle-basics)
- [running gradle init generates below files](#running-gradle-init-generates-below-files)
    - [Automation Testing](#automation-testing)
    - [Excercise](#excercise)
- [use clone with ssh, if clone with https is used then you will need user/password ssh key won't work](#use-clone-with-ssh-if-clone-with-https-is-used-then-you-will-need-userpassword-ssh-key-wont-work)
- [Continous Integration](#continous-integration)
    - [Jenkins Setup Steps](#jenkins-setup-steps)
    - [Jenkins Install](#jenkins-install)
- [removing java and installing compatible version of java](#removing-java-and-installing-compatible-version-of-java)
- [setting up config files in yum to install jenkins from it](#setting-up-config-files-in-yum-to-install-jenkins-from-it)
- [to make sure jenkins start whenever the server is rebooted](#to-make-sure-jenkins-start-whenever-the-server-is-rebooted)
- [to start the jenkins](#to-start-the-jenkins)
- [to see the logs](#to-see-the-logs)
    - [Setting up Jenkins Project](#setting-up-jenkins-project)
- [Continous Delivery](#continous-delivery)
    - [Jenkins Pipeline](#jenkins-pipeline)
    - [Stages and Steps](#stages-and-steps)
    - [Deployment Example](#deployment-example)
- [Containers](#containers)
    - [Docket Install](#docket-install)
    - [Docker Basics](#docker-basics)
    - [Docker Commands](#docker-commands)
    - [Dockerfile](#dockerfile)
    - [Installing Docker in Jenkins Server](#installing-docker-in-jenkins-server)
    - [Jenkins Pipelines Using Dockers](#jenkins-pipelines-using-dockers)
    - [Excercise](#excercise)
- [Orchestration](#orchestration)
    - [Creating a K8s Cluster](#creating-a-k8s-cluster)
        - [Installing docker.io, kubelet, kubeadm, kubectl](#installing-dockerio-kubelet-kubeadm-kubectl)
        - [setup using kubeadm](#setup-using-kubeadm)
        - [Finish setup](#finish-setup)
        - [setup networking - fannel](#setup-networking---fannel)
        - [Join the server node](#join-the-server-node)
    - [Kubernetes Basics](#kubernetes-basics)
        - [Definitions](#definitions)
    - [Deploying to Kubernetes with Jenkins](#deploying-to-kubernetes-with-jenkins)
- [Monitoring](#monitoring)
- [Self-Healing](#self-healing)
- [Auto Scaling](#auto-scaling)
- [Canary Testing](#canary-testing)
- [Fully Automated Deployment](#fully-automated-deployment)


# Topics I need to learn

* Change Configuration (Chef, Puppet, Salt Stack, **Ansible**)

* Cloud Native Infrastructure, O'Reilly book on it (service discovery, reverse proxy to service mesh, deployment patterns, web traffic), beyond 12-Factor
* Basic systems engineering and network engineering, database and/or no-sql exposure
* CICD continuous integration-delivery pipeline (**Jenkins**, **Gitlab**)
* Infrastructure As Code(**Terraform**, CloudFormation, 
* Ideally two cloud providers, minimal one (AWS, GCP, Azure), and bonus for hybrid-cloud (OpenStack, vSphere, Cobbler, FAI, etc)
* optional - local development tool-chain (vagrant, docker-compose, test-kitchen)

Basically, learn how to build, test, deploy a cloud application and supporting infrastructure, and run the testing and delivery through a CI tool. All the concepts come into play.

# Building Packaging and Deployment
```
Building is compiling all of the software. The build process usually builds the main program/s as well as its libraries. Compiling takes computer code and translates it into machine code. Compilation is always targeted toward specific "architectures" usually 32-bit or 64-bit, depending on what machines you intend to use the software on. The compilation process is also performed per OS so you might have a 32 bit windows version, 64 bit windows version, 32 bit linux ubuntu version, etc. The build process can include a single build like 32bit windows or all builds.

Packaging involves how you intend to transport your software (executables, libraries, resource files, folder structures, etc) to the end user. This might mean you create an installer, or just pack everything into a zip file or something. The package process gets the software ready for deployment.

Deployment is the final step of the software getting where it is meacd nt to be and getting installed and running.This can be emailing your zipped software to someone with written instructions of how to unzip and run, or uploading it to a web server for download, or installing it on a local server, etc, etc.
```

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

* Step 1-3: Setup git

```s
$ git config --global user.name "name"
$ git config --global user.email "email"
$ ssh-keygen -t rsa -b 4096 # create ssh key -t is to chose the algorithm in this case rsa, -b is size in this case 4096
$ cat /home/user/.ssh/id_rsa.pub # copy key and paste it in github account
$ git clone git@github.com:peoplesoftninja/cicd-pipeline-train-schedule-gradle.git
# use clone with ssh, if clone with https is used then you will need user/password ssh key won't work
```
* Step 4: Install gradle `./gradlew build` This creates build.gradle and other files
* Step 5-9: Edit `build.gradle`
  

build.gradle
```javascript
plugins {
  //inlude the nodeJS plugin to execute nodejs and npm tasks
  id "com.moowork.node" version "1.2.0"
}

node {
	download = true //this downloads node
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
* Step 9: Run `./gradlew build` this will create the dist folder which will have trainSchedule.zip
* Step 10: Git add,commit,push

* If we clone a new project which has ./gradlew file then we don't have to install gradle, we can just use the above command and it will take care of itself. But it is important to have java installed. 
  
# Continous Integration

* CI is all about the practice of frequently merging code changes
* But, building the code manually will be a lot of work if we do frequent merge. So we automate it
* So CI is all about the practice of merging code but to fascilate it we automate Build process
* A CI server executes a build that automatically prepares and compiles the code and run automated tests
* usually, this is trigger by code changes at the source control
* If something goes wrong, the build fails and the team gets immediate feedback, via server sending email or slack message
* We use Jenkins as an Automation tool. `The leading open source automation server, Jenkins provides hundreds of plugins to support building, deploying and automating any project`

## Jenkins Setup Steps


* Install Jenkins
* Install Plugins
* Set Up Git Hub
    * Manage Jenkins
    * Add Git Server
    * Get `Personal Access Token` From Github Page, Available in Setting/DeveloperSettings. Generate New Token, select admin:repo_hook, copy key
    * Paste key as Secret Text give user ID, password and add,save.
* Create a New Item -- Free Flow
    * If Free Flow, Add Git Hub--use username and password for username github user and password will be the ssh key, generation see above
    * Build Triggers, Check Github hook triggers for GITScm Polling, Build, Select the step necessary. If Gradle, Use Gradle Wrapper, and give the task, usually build
    * Post Build, use the directory as well `dist/trainSchedule.zip`
* Create a New Item - Pipeline
    * Download `pipelin` and `deploy over ssh` plugins in jenkins
    * Go to Configure--Publish Over Ssh--Name(staging/production)--hostname(ip)--Root Directory(/)
    * Don't give credentials here, Rather, go to credentials--Jenkins Scope--Global--add Credentials--add username and password which you have created in the server--the id is what you will use to access the credentials in the pipeline
    * New Item--Multi Branch Pipeline
    * Banch Github--setup see above
    * In the owner give username of github and repository list will be available, select the repository you are working on. As soon as you save it will initiate a build. In Free flow you need to click it manually
    * In Jenkins File add build, deployment stages, see example [here]((#deployment-example))

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

## Stages and Steps

* Jenkinspipeline is divided into stages and steps
* Stages are large peices of CD process
* Eg: Build the code, Test the code, Deploy to staging, Deploy to Production
* Steps are individual tasks that make up each stage
* Eg: Execute a command, Copy files to server, Restart a service, Wait for input from human
* Steps are added though special declarative keyword in the jenkins file DSL, plugins can add steps too.
* [steps documentation](https://jenkins.io/doc/pipeline/steps/)

```js
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    }
}
```

## Deployment Example

* Continious Delivery means *ensuring* you are always able to deploy *any version* of your code. This is a pre-req to Continious Deployment
* Continous Deployment means you are actually *deploying* your code frequently

* We do the following
    * Create 2 servers, one is staging and the other in Prod
    * In Jenkins download necesseary plugins
    * Configure Jenkins to connect with github repo
    * Create a New item  for MultiBranch Pipeline
    * Define stages in Jenkinsfile
    * Define steps in Jenkinsfile
    * prompt a user for approval before moving to prod ( this can be removed in the future in Jenkinsfile

* First step is to create the Stage(DEV) and Prod Servers where we will deploy the code.
* Creating CentOS server, run the below commands as sudo

```s
adduser deploy
echo "deploy:jenkins" | chpasswd
groupadd train-schedule
usermod -a -G train-schedule deploy
echo "deploy ALL=(ALL) NOPASSWD: /usr/bin/systemctl stop train-schedule" >> /etc/sudoers
echo "deploy ALL=(ALL) NOPASSWD: /usr/bin/systemctl start train-schedule" >> /etc/sudoers
echo -e "[Unit]\\nDescription=Train Schedule\\nAfter=network.target\\n\\n[Service]\\n\\nType=simple\\nWorkingDirectory=/opt/train-schedule\\nExecStart=/usr/bin/node bin/www\\nStandardOutput=syslog\\nStandardError=syslog\\nRestart=on-failure\\n\\n[Install]\\nWantedBy=multi-user.target" > /etc/systemd/system/train-schedule.service
/usr/bin/systemctl daemon-reload
mkdir -p /opt/train-schedule
chown root:train-schedule /opt/train-schedule
chmod 775 /opt/train-schedule/
yum -y install nodejs unzip
```
* the first 2 echo appends the command in `/etc/sudoers` file which basically gives permission to user `deploy` to run the command
* The third echo creates the file as shown below

/etc/systemd/system/train-schedule.service
```s
[Unit]
Description=Train Schedule
After=network.target
[Service]
Type=simple
WorkingDirectory=/opt/train-schedule
ExecStart=/usr/bin/node bin/www
StandardOutput=syslog
StandardError=syslog
Restart=on-failure
[Install]
WantedBy=multi-user.target
```

* Make sure all plugins are available
    * Installing 2 Plugins: `Publish Over ssh` and `Pipeline`
    * Manage Jenkins -- Install Plugins -- Available --Search for Publish over ssh ( To transfer files to servers DEV/Stage/PRD)--Search for Pipeline and install that plugin as well
* Add the DEV and PRD servers. 
    * Manage Jenkins -- Configure System -- Publish Over Ssh (Add SSH Servers) Name:Stage/Prd, Hostname: url, Remote Directory:`/` (Don't add credentials here, leave user empty )
* Add Credentials 
    * Credentials--Stores scoped to Jenkins--System(Global Credentials)--Add Credentials--kind(username with pass)--username/password/ID(you will use this in the pipeline file) 

* Creating Pipeline
* Create New Item--Name--Multibranch pipeline--Branch Source(Github)--Credentials(Add)(use username/passw as ssh won't work here, username is github user and password is ssh key)
* Add the credential--owner is github account--Repos of your account will be added(select the required one)--save
* As soon as you save jenkins will start building all the branches, since that is the only stage it will only build.


* Editing the `Jenkinsfile` to add the pipeline
Jenkinsfile
```groovy
pipeline {
    agent any //tells which stage to run, any will run all stages
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon' //shuts off jenkins daemon, this is good practice in Jenkins file to optimize the process
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master' // this means it will only run in master branch and in no other branch
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) { //access credentials from jenkins and stores user/pass values as reference to access below
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false, // won't let deploy further if something goes wrong, makes code more robust
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging', //server config from jenkins see setup above
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip', //transfering from jenkins got generated in build stage
                                        removePrefix: 'dist/', // without this the dist/ prefix is also used as a zip filename
                                        remoteDirectory: '/tmp', //in the sever stored here
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                input 'Does the staging environment look OK?' // gives a pop up to proceed to abort before deploying to prod
                milestone(1) //if another build is running it will make sure it finishes and then this is run
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}

```

* Once this is done, and gets build. Use the `serverip:3000` to see if website is deployed

# Containers

## Docket Install

```s
$ sudo yum install docker
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo docker info
```

## Docker Basics

* Image: 
    * Package of the software and everything it needs in order to run
    * They exist in an heirarchy. Parent image and every new config or functionality adds a new layer
    * When we create an image for our app, we will use an image which already has 90% of the confi and just add our app layer to it
* Container: An instance of an image
* Dockerfile: Place where image is defined
* Docker build: to create the image from the dockerfile
* Docker run: To create a container instance from the image
* Docker Registry: Like Github, DockerHub, you can create your personal registry Using a container
    * A place to store images, like github
    * `docker build` to build image and `docker push` to push image to registry, then you can do `docker pull` and `docker run` from anywhere that is setup to access that registry
    * You can maintain your own registry(running as a docker container) or use official cloud registry `hub.docker.com`. If free then all public, if you want private, pay.
    * To authenticate docker hub `sudo docker login --username=<username> --email=<email>`
  
## Docker Commands
* To authenticate docker hub `sudo docker login --username=<username> --email=<email>`
* When you build an image you give it a name and optional tags 
    * `sudo docker build -t <docker user name>/<docker image-name> .`
* Once we build the image we create and run a container instance 
    * `docker run -d <docker user name>/<docker image-name>` This will run container in the background and print container id d is for detatch
    * `docker run -p port:port -d <docker-username>/docker image-name` p is for port opening
    * `docker container run <docker user name>/<docker image-name>` This will start the container do the needfull and then end it
    * `docker container run <image-name> command` will start the container run the command give the output and stop the container
    * `docker container run -it <image-name> /bin/sh`  Launches shell to get out `exit`
* To see a lit of containers
    * `docker container ls` or `docker ps`
* To see list of active/inactive containers
    * `docker container ls -a`
* Start/Stop Container
    *  `docker container start <container-id>` and then execute it by `docker container exec -it <container-id>`
    * `docker stop <container id>` to stop a cotainer
* To pull Images
    * `docker image pull <name>`
* To push 
    * `docker push`
* To see a list of images
    * `docker image ls`
* `docker run --restart always` will make sure docker is always running. This is how we run docker in PROD. This way if app fails, docker will restart it.

## Dockerfile

* `FROM <image name>` - sets the parent image
* `WORKDIR <directory path>` - Sets the current working directory inside the container image for other commands
* `COPY <source> <desitnation>` copies files from the host into the container images
* `RUN <command>` - executes a command within the container image
* `EXPOSE <port>` - tells docker that the software in the container listens on a particular port
* `CMD <array of command arguments>` - sets the command that is executed by the container when it is run

```Dockerfile
FROM node:carbon # official node image
WORKDIR /usr/src/app # working directory in the container
COPY package*.json ./ # just copying one file first
RUN npm install # this will install node from the package.json
COPY . . # copying all files
EXPOSE 8080 # opening port
CMD ["npm", "start"] # container will run this whenever we start it
```

Website will be accessable from host server:8080 port, we will ahve to map it. 

Commands to run
```s
$ sudo yum install docker
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo docker info
$ sudo yum groupinstall -y "development tools"
$  ssh-keygen -t rsa -b 4096
$ take a fork of the train-schedule-docker app, and add the above key to github ssh keys
$ sudo cat /home/user/.ssh/id_rsa.pub
$ git clone git@github.com:peoplesoftninja/cicd-pipeline-train-schedule-docker.git
$  cd cicd-pipeline-train-schedule-docker/
$ vim Dockerfile
$ sudo docker build -t peoplesoftninja/train-schedule .
$ sudo docker run -p 8080:8080 -d peoplesoftninja/train-schedule
```
## Installing Docker in Jenkins Server

```s
sudo yum -y install docker
sudo systemctl start docker
sudo systemctl enable docker
sudo groupadd docker # Creating a group called docker
sudo usermod -aG docker jenkins # Adding jenkins to that group, this way we jenkins user can access docker
sudo systemctl restart jenkins # restarting jenkins and dockers
sudo systemctl restart docker
```

## Jenkins Pipelines Using Dockers

Steps to Dockerize the Deployment Process

* Jenkins produces a Docker Image with the new code
* Jenkins pushes the new image to docker Hub
* Jenkins pulls the new image on the production server
* Jenkins stops the container running the old code
* Jenkins runs `docker run` on the server to run the new image


## Excercise

Your team is transitioning from a traditional static deployment model to using containers. They hope to eventually implement container orchestration, but right now they want to take the step of running the train-schedule app in production as a Docker container. The team also wants to maintain an automated CI/CD process through this transition. A Dockerfile has already been created for the app which can be used to build Docker images for it, and a production server has been set up with Docker installed.

The team has asked you to modify the Jenkins Pipeline and configure Jenkins so that the Docker image can be built and deployed as part of the Jenkins CD Pipeline. You will need to do the following:

* Configure Jenkins credentials for the production server.

Log into Jenkins--Credentials--Jenkins--Global Credentials--Add Credentials (ID is what is used in Jenkinsfile)

* Configure Jenkins credentials for the Docker image registry (Docker Hub).

Same as above

* Configure a global property in Jenkins to store the production server IP.

Manage Jenkins--Configure System--Global Properties--Add Environment Variable--Name/Value(host)--Name is used in Jenkinsfile

* Create a multibranch pipeline project in Jenkins called train-schedule.

In Github, fork the project--settings--developerSettings--PersonalAccessTokens--GenerateNew--Select admin:repo_hook--copy it

New Item--MultiBranch Pipeline--Add Github--in add crednetials, give user name as github user and password as sshkey, save it, select repository which we are working, save the item

* Configure the Jenkins project to pull from your fork of the source code automatically

Manage Jenkins--Configure System--Add Github server--credentials use ssh text, paste ssh key--save. In github repo, go to settings, check for webhooks, see if webhook is present, if not, in jenkins, go to advance and click Re-register hooks for all jobs

* Stages to the Jenkins Pipeline (Jenkinsfile) to do the following:
Build a Docker image with the code inside it.
Push the image to Docker Hub.
Deploy the new image to production: stop and remove any older containers, pull and run the newly built image.
You will need a Docker hub account in order to use Docker Hub as an image registry. You will need to enter your credentials into the lab environment Jenkins instance, so feel free to create a fresh Docker Hub account and cancel it after you are done.

You will need to create a personal fork of the source code here: https://github.com/linuxacademy/cicd-pipeline-train-schedule-dockerdeploy

Check out the Jenkinsfile on the example-solution branch for an example of how this pipeline can be implemented. If you use that code, however, be sure to change all references to the willbla/train-schedule Docker image name to a name that is unique to your Docker Hub account, such as <your Docker Hub username>/train-schedule

```jenkinsfile
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image'){
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("peoplesoftninja/train-schedule3")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Pushing Docker Image'){
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_id'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('Deploy to Production'){
            when {
                branch 'master'
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                withCredentials([usernamePassword(credentialsId:'web_server_id', usernameVariable:'USERNAME', passwordVariable:'USERPASS')]){
                    script{
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull peoplesoftninja/train-schedule3:${env.BUILD_NUMBER}\""
                        try {
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stop train-schedule\""
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker rm train-schedule\""
                        } catch (err){
                            echo: 'caught error:$err'
                        }
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker run --restart always --name train-schedule -p 8080:8080 -d peoplesoftninja/train-schedule3:${env.BUILD_NUMBER}\""
                    }
                }
            }
        }
    }
}
```
# Orchestration

* Orchestrates pieces of automation, by automating them in a workflow and processes to manage application and infrastructure

* Some examples 
    * I sclae up by asking my orchestration tool for 10 new instances
    * My Orchestration automatically scales up in response to load
    * My orchestration tool automatically destroys and replace broken node

* Kubernetes(k8s) is the orchestration tool. For more info go [here](https://kubernetes.io/)

## Creating a K8s Cluster

### Installing docker.io, kubelet, kubeadm, kubectl

* Install docker.io

```s
$ sudo yum install docker
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo docker info
```
....TOTO, watch [this](https://www.youtube.com/watch?v=6xJwQgDnMFE) video, to install

### setup using kubeadm

* create a config file, ` vim kube-config.yml`

kube-config.yml
```yml
apiVersion: kubeadm.k8s.io/v1alpha3
kind: InitConfiguration
networking:
  podSubnet: 10.244.0.0/16
apiServerExtraArgs:
  service-node-port-range: 8000-31274 # for production nodes you don't want to keep all open
```
* install by running `sudo kubeadm init --config kube-config.yml`
    * alternately, you can also do it without the config file by `sudo kubeadm init —-pod-network-cidr=10.244.0.0/16`
    * if you find later that you need to change the port range, you can modify the kube-apiserver.yaml file, adding the --service-node-port-range. For example, `sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml`

output
```s
[init] using Kubernetes version: v1.12.1
[preflight] running pre-flight checks
[preflight/images] Pulling images required for setting up a Kubernetes cluster
[preflight/images] This might take a minute or two, depending on the speed of yo                               ur internet connection
[preflight/images] You can also perform this action in beforehand using 'kubeadm                                config images pull'
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/                               kubeadm-flags.env"
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[preflight] Activating the kubelet service
[certificates] Generated etcd/ca certificate and key.
[certificates] Generated etcd/healthcheck-client certificate and key.
[certificates] Generated etcd/server certificate and key.
[certificates] etcd/server serving cert is signed for DNS names [salmanzaidy5.my                               labserver.com localhost] and IPs [127.0.0.1 ::1]
[certificates] Generated etcd/peer certificate and key.
[certificates] etcd/peer serving cert is signed for DNS names [salmanzaidy5.myla                               bserver.com localhost] and IPs [172.31.97.188 127.0.0.1 ::1]
[certificates] Generated apiserver-etcd-client certificate and key.
[certificates] Generated ca certificate and key.
[certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [salmanzaidy5.myla                               bserver.com kubernetes kubernetes.default kubernetes.default.svc kubernetes.defa                               ult.svc.cluster.local] and IPs [10.96.0.1 172.31.97.188]
[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
[certificates] Generated sa key and public key.
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.                               conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
[controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/k                               ubernetes/manifests/kube-apiserver.yaml"
[controlplane] wrote Static Pod manifest for component kube-controller-manager t                               o "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/k                               ubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/m                               anifests/etcd.yaml"
[init] waiting for the kubelet to boot up the control plane as Static Pods from                                directory "/etc/kubernetes/manifests"
[init] this might take a minute or longer if the control plane images have to be                                pulled
[apiclient] All control plane components are healthy after 21.502543 seconds
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in t                               he "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.12" in namespace kube-system wi                               th the configuration for the kubelets in the cluster
[markmaster] Marking the node salmanzaidy5.mylabserver.com as master by adding t                               he label "node-role.kubernetes.io/master=''"
[markmaster] Marking the node salmanzaidy5.mylabserver.com as master by adding t                               he taints [node-role.kubernetes.io/master:NoSchedule]
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to t                               he Node API object "salmanzaidy5.mylabserver.com" as an annotation
[bootstraptoken] using token: y3j4k3.5iw2ap9u3gt4itf5
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CS                               Rs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller autom                               atically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all nod                               e client certificates in the cluster
[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" name                               space
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 172.31.97.188:6443 --token y3j4k3.5iw2ap9u3gt4itf5 --discovery-to                               ken-ca-cert-hash sha256:1517c541cd89c17e697d782c1c740813db05ba9b3c9aff8cd5b4ca41                               86814057

```
### Finish setup

* after this if you run `kubectl get nodes` it won't work as k8s is not configured properly. For this run the commands

```s
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

* now when you do the `kubectl get nodes` you will get results

```s
NAME                           STATUS     ROLES    AGE    VERSION
xyz5.mylabserver.com   NotReady   master   117s   v1.12.1
```
### setup networking - fannel

* `kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml`
* You will get following result
```s
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.extensions/kube-flannel-ds created
```

* create the `10-flannel.conflist` file
```s
mkdir -p /etc/cni/net.d
cat << EOF > /etc/cni/net.d/10-flannel.conflist
{
"name": "cbr0",
"plugins": [
{
"type": "flannel",
"delegate": {
"hairpinMode": true,
"isDefaultGateway": true
}
},
{
"type": "portmap",
"capabilities": {
"portMappings": true
}
}
]
}
EOF
```

### Join the server node

* Login to the server node 
* run the above join command `sudo kubeadm join 172.31.97.188:6443 --token y3j4k3.5iw2ap9u3gt4itf5 --discovery-token-ca-cert-hash sha256:1517c541cd89c17e697d782c1c740813db05ba9b3c9aff8cd5b4ca4186814057`
* From the master node run `kubectl get nodes`

* create the `10-flannel.conflist` file as above

* `kubectl get nodes` should give the list 

## Kubernetes Basics

### Definitions

* Cluster - The entire K8s system, a master and one or more nodes
* Master - Runs Control Process and K8s API
* Node - A worker Machine
* Pod - A runnign process on a node. For example, a single container or a group of containers
* Service - An abstraction defining a set of pods(across nodes) and a way to access them

## Deploying to Kubernetes with Jenkins

* Use `Kubernetes Continious Deploy` Plugin in Jenkins
* After installing, go to `Credentials` in that select `Kubeconfig`, give id and description and select Enter directly. You need to paste the config details from the kubernetes Master. 
* `cat ~/.kube/config` copy the entire contents
* In Jenkins create a new item and select Multi Branch Project, synch the git hub repo and save
* Create a new file `train-schedule-kube.yaml`
```yml
kind: Service
apiVersion: v1
metadata:
  name: train-schedule-service
spec:
  type: NodePort
  selector:
    app: train-schedule
  ports:
  - protocol: TCP
    port: 8080
    nodePort: 8080

--- ## defining multiple documents in a single yaml file

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: train-schedule-deployment
  labels:
    app: train-schedule
spec:
  replicas: 2
  selector:
    matchLabels:
      app: train-schedule
  template:
    metadata:
      labels:
        app: train-schedule
    spec:
      containers:
      - name: train-schedule
        image: $DOCKER_IMAGE_NAME:$BUILD_NUMBER
        ports:
        - containerPort: 8080

```
* edit the jenkins file
Jenkinsfile
```
pipeline {
    agent any
    environment {
        //be sure to replace "willbla" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "willbla/train-schedule"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true // replacing $ values of the above yml file
                )
            }
        }
    }
}

```

# Monitoring

* Collecting and Preseting data about the performance and stability of applications and infrastructure
* Two Major Types of Data
    *   Infrastructe Monitoring:- Collecting data about the health, performance and capacity needs of infrastructure: CPU Load, Memory Usage, Disk Usage etc
    *   Application Performance Monitoring APM:- Collecting ata about the health and performance of the apps tehmselves: Requests per minute, error rate, average response time, etc
*  Once we have data we can do two things
    *  Aggregation: Usually means displaying the data in the form of grasphs and dashboard
    *  Alerting:- Letting us know real time to repsonse
* We will work with two popular monitoring tools
    * Prometheus: For Gathering and Storing Monitoring data. It also provides alerting
    * Grafana: Will let us display and build dashboards for the data

# Self-Healing
# Auto Scaling

# Canary Testing

2-3

# Fully Automated Deployment

1-2
