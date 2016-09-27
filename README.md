# DevOps-Milestone1: Configure build Server

Team Members:

1. Ayush Gupta
2. Nishtha Garg
3. Shivam Gulati

We have used Jenkins to configure the build server. It would also respond autmatically to post commit events and send build status emails. Alongwith Jenkins, we have used Tomcat, Git for Source Code Management and NPM as a package manager.

### Initial Jenkins Setup

#### Step 1: Install Jenkins

1. Installation on Mac
[Detailed Instructions for Mac](https://wiki.wocommunity.org/display/documentation/Installing+and+Configuring+Jenkins)
2. Installation on Linux/Ubuntu
[Detailed Instuctions for Linux/Ubuntu](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)

You will be required to enter the admin password once to start jenkins, which can be found in secret of your Jenkins system directory folder but you may need to first give it permissions to unlock by modifying properties.

#### Step 2: Configure Jenkins

1. Plugins requirement
   1. Go to Manage Jenkins->Manage Plugin->Install NodeJS Plugin.
   2. Make sure you already have Github and Email plugin installed, else install them as well.
2. Go to Global Tool Configuration-> Select NodeJS install automatically-> Give any Name and Save.
3. Go to Manage Jenkins-> Configure Global Security. Under Authorization, choose, "Anyone can do anything". Save.

#### Step 3: Creating a New Job

1. Create a new job. Give it a name. Choose Freestyle Project.
2. For configuration of Job
   Source Code Management-> select Git under Source Code Management-> Enter your project's Git Repository URL.
3. Under Build Environment-> select "Provide Node & npm bin/ folder to PATH"-> choose the NodeJS installation.
4. Additionally, you can also setup multiple branches for the same job.
5. Click Save.
5. Test run a build - Go to Jobs and click "Build Now". The progress would be depicted as an animated circle. Blue implies a successful build and Red implies a failure.

### Build Server Tasks

#### TASK 1: Ability to trigger a build in response to git commit via git post-commit hook

1. When the user creates a new commit to the repository, Git executes the script created for post-commit hook.
2. We trigger the build in Jenkins whenever user does a commit. Our script looks like:

```
#!/bin/bash

curl http://<username>:<user-token>@localhost:8080/job/<job-name>/build?token=<job-token>
```

#### TASK 2: The ability to execute a build job via a script or build manager which ensures a clean build each time

1. Select your job and go to "Configure" option from the left pane.
2. In General tab, under Build sub-tab, select "Add a Build Step".
3. Choose "Execute Shell"
4. Here, you can write your script to be executed for Build process. We use NPM to install modules and run tests during each build.

```
rm -rf node_modules
npm install
npm test
```

#### TASK 3: The ability to determine failure or success of a build job and trigger an external event  by email

#### TASK 4: The ability to have multiple jobs corresponding to multiple branches in a repository

#### TASK 5: The ability to track and display a history of past builds

Sources:

1. https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins
2. https://www.nczonline.net/blog/2015/10/triggering-jenkins-builds-by-url/

#### Screencast

![Screencast]()

[Link to GIF] ()
