# DevOps-Milestone1: Configure build Server

Team Members:

1. Ayush Gupta - agupta25@ncsu.edu
2. Nishtha Garg - ngarg@ncsu.edu
3. Shivam Gulati - sgulati2@ncsu.edu

We have used Jenkins to configure the build server. It would also respond autmatically to post commit events and send build status emails. Alongwith Jenkins, we have used Tomcat, Git for Source Code Management and NPM as a package manager.

### Build Section

#### Step 1: Install Jenkins

1. Installation on Mac
	[Detailed Instructions for Mac](https://wiki.wocommunity.org/display/documentation/Installing+and+Configuring+Jenkins)
2. Installation on Linux/Ubuntu
	[Detailed Instuctions for Linux/Ubuntu](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)

#### Step 2: Other setups

1. Install Tomcat 8.
	[Guide for Instructions](https://www.ntu.edu.sg/home/ehchua/programming/howto/Tomcat_HowTo.html)
2. Make sure you have NPM or or project managers install as per your requirement.
3. Once everything is setup, open web browser and go to URL http://localhost:8080/ to run Jenkins.
4. You will be required to enter the admin password once to start jenkins, which can be found in secret of your Jenkins system directory folder but you may need to first give it permissions to unlock by modifying properties.

#### Step 3: Configure Jenkins

1. Plugins requirement
   1. Go to Manage Jenkins->Manage Plugin->Install NodeJS Plugin.
   2. Make sure you already have Github and Email plugin installed, else install them as well.
   
   ![Screenshot](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/Plugin%20screen.JPG)
2. Go to Global Tool Configuration-> Select NodeJS install automatically-> Give any Name and Save.
3. Go to Manage Jenkins-> Configure Global Security. Under Authorization, choose, "Anyone can do anything". Save.

#### Step 4: Creating a New Job in Jenkins

1. Create a new job. Give it a name. Choose Freestyle Project.
	
	![Screenshot](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/NewJob.JPG)
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

curl http://<username>:<user_token>@localhost:8080/job/<job_name>/build?token=<job_token>
```
![Screencast](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/1.gif)


#### TASK 2: The ability to execute a build job via a script or build manager which ensures a clean build each time

1. Select your job and go to "Configure" option from the left pane.
2. In General tab, under Build sub-tab, select "Add a Build Step".
3. Choose "Execute Shell"
4. Here, you can write your script to be executed for Build process. We use NPM to install dependencies modules from package.json file and run tests during each build.

```
rm -rf node_modules
npm install
npm test
```
![Screencast](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/2.gif)


#### TASK 3: The ability to determine failure or success of a build job and trigger an external event  by email.

1. Go to Manage Jenkins -> Configure System.
2. Setup Extended E-mail Notification and E-mail notification.

	![Screenshot](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/Email1.JPG)
3. For both of the above, we configured gmail smtp server. 
4. Enter the SMTP details and credentials and click Save.
5. Follow below screenshots for reference.
   ![Configure Email 1]()
   ![Configure Email 1]()
6. Go the Configure page of your job-> Post-build Actions-> Post Build Action-> Choose Editable Email Notification.

	![Screenshot](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/PostBuildAction.JPG)
7. Now, when you build you will get the email notification for the corresponding build.

Note: If the recipient is @gmail, you need to turn on the security access by using the link https://www.google.com/settings/security/lesssecureapps

![Screencast](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/3.gif)


#### TASK 4: The ability to have multiple jobs corresponding to multiple branches in a repository

1. We saw earlier how to create a job.
2. Create multiple jobs using the same procedure and with different branches. In this case, the branch are **dev** and **release**.
3. We have two jobs, **demo1-dev** for dev branch and **demo1-release** for release branch.
4. We use the existing post-commit script and modify it to trigger builds as per the branch we are working on.

```
#!/bin/bash

if [ `git rev-parse --abbrev-ref HEAD` == "dev" ]; then
	curl http://<username>:<user_token>@localhost:8080/job/<dev_job_name>/build?token=<job_token>
elif [ `git rev-parse --abbrev-ref HEAD` == "release" ]; then
	curl http://<username>:<user_token>@localhost:8080/job/<release_job_name>/build?token=<job_token>
fi
```

![Screencast](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/4.gif)


#### TASK 5: The ability to track and display a history of past builds (a simple list works) via http

1. The entire build history is visible in Jenkins. Users can also follow the below URL to view the build history over HTTP.
2. Replace **<job_name>** by actual job name setup in Jenkins.

```
http://localhost:8080/job/<job_name>/api/json?pretty=true
```

![Screencast](https://github.com/shivamgulati1991/DevOps-Milestone1/blob/master/Screens/5.gif)


Sources:

1. https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins
2. https://www.nczonline.net/blog/2015/10/triggering-jenkins-builds-by-url/
