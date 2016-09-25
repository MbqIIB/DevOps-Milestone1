# DevOps-Milestone1: Configure build Server

Name: Shivam Gulati

Email: sgulati2@ncsu.edu

We have used Jenkins to configure the build server. It would also respond autmatically to post commit events and send build status emails.

#### Step 1: Install Jenkins

1. Installation on Mac
[Detailed Instructions for Mac](https://wiki.wocommunity.org/display/documentation/Installing+and+Configuring+Jenkins)
2. Installation on Linux/Ubuntu
[Detailed Instuctions for Linux/Ubuntu](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)

You will be required to enter the admin password once to start jenkins, which can be found in secret of your Jenkins system directory folder but you may need to first give it permissions to unlock by modifying properties.

#### Step 2:Configure Jenkins

1. Plugins requirement
..1. Go to Manage Jenkins->Manage Plugin->Install NodeJS Plugin.
..2. Make sure you already have Github and Email plugin installed, else install them as well.
2.Go to Global Tool Configuration-> Select NodeJS install automatically-> Give any Name and Save.
3.Go to Manage Jenkins-> Configure Global Security. Under Authorization, choose, "Anyone can do anything". Save.

#### Step 3: Creating a New Job

1.Create new job. Give it a name. Choose Freestyle Project.
2.For configuration of Job:
Source Code Management-> Select Git-> Enter the Git Repository URL.
3. Under Build Environment->check 'Provide Node & npm bin/ folder to PATH'-> choose the NodeJS installation

Sources:

1. https://en.wikipedia.org/wiki/Amazon_Web_Services
#### Screencast

![Screencast]()

[Link to GIF] ()
