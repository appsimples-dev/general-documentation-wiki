## 1. Introduction

Working in any development process can be a quite complex task by its own, and when teamwork is involved many problems and difficulties can arise. This guide's main purpose is to aid the team members of any development project in the effort of handling and minimizing those difficulties, by exposing the main methodologies we use here at Outsmart.

In this guide we will start by talking about Git usage, along with BitBucket, our Git control and code review tool. Next we will talk about our team structure, the role of each team member, and their duties. We will then display and explain our main project management guidelines, the Scrum, which is an agile methodology, along with all the routines, processes and concepts that are involved in the Scrum cycle, such as Daily Scrum and Sprints for instance. Also we will present to you Jira, our tool for keeping track and organizing the Scrum tasks.

## 2. Teams

Here at Outsmart we organize ourselves in autonomous teams. The reason for separating our people in teams is that We have a large number of projects that can be overwhelming for a single person to understand in detail. Creating teams and allocating each project for a single team simplifies the processes of managing those project’s responsibilities. Also, in a large team, it can be hard to maintain the horizontal hierarchical system adopted by our company.

Although we divide ourselves in teams, it doesn’t mean we can’t interact with people outside from other teams. We should be communicating and interacting in a company level about organizational and technical issues. Please make an effort to be open to other people, even when they are not from your team.

### 2.1 Autonomy and Responsibility

Each team must have autonomy to complete its projects from start to end. That means it must not depend on people or systems outside of the team’s control to make the decisions and complete the tasks necessary to finish the project. Of course, the team can request help from other people on a specific situation, but it must not rely on it on a regular basis. If your team is systematically dependent on someone from outside to achieve progress in its projects, please raise the issue inside the team and with the company's administration.

With that autonomy comes full responsibility about the project’s result. It is important to keep track of the progress of the project and always communicate issues inside and outside the team. The company will always be open to the team's requests, and try as best as we can to help, but it is the team's responsibility to follow-up about problems with the project. It is also the team's responsibility to identify if there is a personal problem inside it. It has autonomy to resolve those issues inside it, but in a more critical situation it may request for intervention from the company’s administration.
The team as a whole will be 

### 2.2 Organization and Roles
	
We adopt a “flat” structure inside our teams. That means that there is no formal leadership inside them. Everyone in the team is responsible for the team result, and every person’s opinions are equal in the team. We try to make decisions based on data and evidence, so there is no reason for one person’s opinion to be more valuable than others by an hierarchical definition. Instead we trust that each person in the team will be sensible to make decision that will benefit the company's best interest and not their own. When a more controversial or important decision must be made, we can call for a voting process inside the team (this process is described in the item voting).

Each team must have a Product Owner (PO). The PO’s role is to centralize the projects definitions and organizational process. It is important to not mistake the PO for a team or project leader. The POs opinion is not more valuable or important. It is important for the PO, as a centralized source of information, to be extra vigilant about the team's organizational and communicational issues. For more information about the PO, check the Scrum chapter in this document.

Our teams must be multidisciplinary to have autonomy on its projects. If a project requires a specific capability or technological application, the team must provide that requirement. When some of the requirements of the project cannot be met by the team, it must either train someone inside the team for that capability or request for someone from outside the team that has that ability. The company may need to hire someone from outside, but it will be more common to make temporary (or permanent) changes in the teams rosters to best meet the project's requirements. Some of the disciplines (such as design) can be shared between the teams, but that is a situation that is not optimal and must be resolved by the company.

In practice, the teams can establish its own organization (and has autonomy to do so). For instance, one of the projects may require a lot of effort in a specific programming language, so the team can temporarily appoint the most experienced developer in that language as a technical leader for that project.

### 2.3 Voting process

It is important to know when you are not the most knowledgeable person in a discussion, and when the team is trusting you to take the lead in a technical discussion. 

## 3. Git

When it comes to managing and organizing your code in a software development project, using Git is a must. Since it also facilitates code integration in a way that many people can be working on the same code at the same time, Git is very popular for team development. Here at Outsmart we try to make the most out of it, and for that we ask that our employees read our guides carefully in order to understand and comply to the best practices. This section simply scratches the surface of the concepts of our Git methodology, so we ask that for more detailed information along with all the Git commands the reader should refer to our Gitflow Guide.

Git is a version control system, with it you can keep track of all your project's change history and even get back to a point in your project if you wish, in case you detected a major issue, for instance, and instead of spending a lot of time repairing it you could simply go back to a point before that mistake was made. These points are called "commits", and as the name indicates they are points in which you have committed your work to your project.

A Git repository (repo) is represented by its development tree, that may have one or many development branches. A branch is basically a timeline of changes, or commits, it may have its own child branches which almost always return (or merge) back to the main branch. The main branch (or the tree's trunk) is called the master branch, and it contains the project's functional code. A developer is expected to create branches from the master branch (such as feature or bug-fixing branches), work on the project, commit their work in the branch, and then ask for permission to merge the branch back to the master branch, at which point a code review should be done.

[[images/Manual_De_Desenvolvimento/manual_dev_1.png]]
	
At Outsmart we use Bitbucket as our git control and code review tool. Its interface is useful in displaying all of the pertinent Git information, such as our development tree with its branches and commits. Bitbucket holds the project's remote repo, and for a developer to have access to it they must clone it into their machine. Once you clone it, the project will be available as a local repo, and after you have worked on your branch and committed the changes you must push your branch back to the remote repo, so it will be available for everyone in your team to access and review your code. 

If at the point you wish to merge your branch back the master branch hasn't changed, Git will understand that and simply put your changes in the master branch. However, imagine that you worked on your branch and in that period someone has merged their work to the master branch, what do you think can happen? What happens if someone has already changed the very files in which you were working on? Git certainly does not know how to decide for you what parts of your code should be kept instead of the other developer's, so this is why you must always rebase your branch and resolve any conflicts that may have occurred. Rebasing your branch is an operation in which you integrate all the new information that may be present in the parent branch (the master branch in most cases) into the child branch (the branch you're working on), and make it as if your branch has just been created from the parent branch. In this process, Git will show you if there is a conflict between your code and the code present in the master branch. Once again, all this information is detailed in our Gitflow Guide.

After you have pushed your branch to the remote repo, you need to make a request to merge your branch into the master branch, or a pull request (PR). At this point your colleagues can review your code, and even comment on it, giving you suggestions and such, and if everything looks alright, they will approve your PR using Bitbucket's interface. At Outsmart our practice is that it takes at least two developer's approvals to allow a branch to be merged. So, once you have opened a PR and two developers (not including you, of course) have approved your code, you may merge your branch using Bitbucket. Once you have merged your branch, the master branch will now contain all your work, but only at the remote repository. So your next step is to sync your local repo by pulling the master branch from the remote repo, and then you can start your working cycle again. To help you better understand this cycle we will list the most important steps ahead, which should be done for every new feature you will implement.

* If you don't have the project's repo in your machine, clone it, otherwise pull the master branch from the remote repo

* Create a new branch from the master branch to begin your work

* After you have finished your work, commit it to your branch

* Changes may have been made to the master branch, so pull it from the remote repo and rebase your branch and resolve any conflicts that may exist

* Push your branch to the remote repo and open a PR

* Once your PR has been approved by at least two developers, you may merge it into the master branch

And that's the gist of it! Don't forget that it is also part of your job to review your colleagues' code. Read the code thoroughly and leave comments when you judge necessary, don't forget that you may need to use their code or do something similar, this is why reading carefully pays off. Take it also as an opportunity to learn from your colleagues, as they present different solutions from the ones you might have come up with. 

## 4. Scrum

We have briefly introduced Scrum as an agile methodology, but what exactly is an agile methodology? The first idea that may come to your mind might be related to speed, as in finishing a project quickly. We urge you that this is not what agile methodology is about. Instead, its main concern is to help software development teams respond quickly to unpredicted changes and adversities that may occur in a project, by the means of segmenting the work into iterative processes, each process adding more to the project towards a complete product. In the context of Scrum these processes are called Sprints, as we will discuss in detail later on.

The main principles related to the Agile Manifest, that originated the agile methodologies (and an Outsmart employee should know very well), are as follows: 

* _Individuals and their interactions over processes and tools_
This means that little does it matter if the team is using high-tech top-notch tools or highly efficient processes if the team members are not on their best working conditions and their communication is not assured and facilitated - team collaboration is the smaller throttle of project development.

* _Functioning software over in-depth documentation_
The highest priority, and one that we take very seriously at Outsmart is to always make sure that we have a working product, specially at the end of a Sprint. Documenting everything thoroughly can be very time consuming, and though it is an important part of the process, should be preferably a more brief one.

* _Customer collaboration over contract negotiation_
At Outsmart we value collaboration, not only between teams and team members but with the customer as well, which is mostly done through the Product Owner (which we will talk about in a bit). Still, it is not always easy to keep contract negotiations to a minimum, since our business depends strongly on it. Customers will very often change their minds during the project once they start seeing their product taking form, or simply because they will come up with new features for their products. So it is unavoidable that we keep those changes tracked in our contractual agreement. It’s the PO’s responsibility to filter these changes keeping in mind what is best for the client and for the team and to negotiate with the client.

* _Respond to changes over following plans_
When developing a product for a client, changes in the project are virtually unavoidable, therefore this fact alone means that spending too much time on planning can be in most cases a big waste of time. Other than that, you should take into account that the further into the project the more information you have about it, thus a better planning will be made. That is why the teams must be ultimately prepared to face and respond to these changes instead of trying to plan all the steps.

Alright, so now you know the basics of the Agile development methodology, now let's get into Scrum more specifically, what it is and what it comprehends. Let's start at the beginning of a project's lifespan, but first we need to introduce real quickly the concept of Product Owner (PO).

### 4.1 Product Owner

The product owner is the center of the most high-level decision making of the project, and is active in all the steps in Scrum. Every Outsmart team should have a Product Owner, if it is not clear who is the PO of your team, please raise the issue with your colleagues.
So, once the project has been set, the product owner will catalogue all of its most obvious and primary features and functionalities and organize them in a list, this list is called the Product Backlog. Some of the tasks that fall under the PO’s responsibilities are:

* Make sure that the project’s features and team objectives are well defined and clear to all team members.

* Communicate with the customer 

Example: if a team member has a question about the project, they should contact the PO. If the PO cannot resolve the issue, it’s his responsibility to contact the customer or another person to resolve. The PO has autonomy to start discussions or meetings to clarify any doubts about the project.

### 4.2 The Sprint

Now a project cycle will begin: the Sprint. We have mentioned the sprint a few times already, and by now you know almost all there is to know about it: it is an iterable process that increments the product, and the result of each sprint is a functional, working product. When we say "working product", it means that the software should have functional features, but of course not all features will be available after a sprint. This ensures, however, that even if a project meets the deadline before it's completed, there will still be a product to ship. 
A sprint should take a short time period, usually one to two weeks. To set off a sprint, the product owner will analyze the product backlog and then assign priorities to the features.

### 4.3 The Sprint Planning

Next, a Sprint Planning will take place, which is a meeting with the team members in which they will, along with the product owner, decide which of the tasks will be fitting for the upcoming sprint, bringing as a result a list of activities called Sprint Backlog. Keep in mind that the product backlog can undergo changes at any time, and like it was said in the fundamentals of the Agile development, the team should be ready for them, but the product owner should make an effort to keep these changes out of an ongoing sprint's backlog. Finally, the project will be complete once enough sprints have been made so that all the tasks in the product backlog have been done.

### 4.4 The Daily Scrum

Everyday during a sprint, the team gathers to perform a Daily Scrum. The daily scrum (or simply daily, as we say at Outsmart) is a meeting with the main purpose of spreading and disseminating information. In this occasion, each team member will share information on what they have done the previous day (or since the last daily) and what are they prioritizing for the next day, this way every member will be updated on the project's status, and it will give a general idea on the next steps that should be taken. This should also be an opportunity for a team member to share any problem or impairment that they are facing, so the other team members may help. That being said, we stress that the daily should be used simply to raise this questions and not for any in-depth discussions or resolutions, which should happen later with only the people that are directly involved. Here at Outsmart as in most cases, in an effort to keep the dailies straight to the point we ask the team members to participate while standing. It is important that all members participate, but of course this won't always be possible. 

### 4.5 The Sprint Review and Retrospective
	
At the end of a sprint, the team meets to present what each of them have done and what functionalities have been implemented (usually accomplished by the use of demos) and at this moment it will be evaluated if the sprint main objectives have been reached. Following the review, during the Sprint Retrospective it should be made an evaluation of the sprint performance. What has worked well? What hasn't? This answers should be used as an input for the next sprint in order to refine the process and attempt to avoid the same problems and obstacles.

Great! If you have read and understood all the Scrum and agile concepts listed in this section you know everything about these subjects that is expected of an Outsmart employee. We do encourage you to read further at the following link:

https://www.scrumalliance.org/why-scrum/scrum-guide

## 5. Jira
	
We have talked about how we use agile Scrum guidelines (if you haven't read this part please do! It's fundamental for the understanding of this section, also the Git section is rather important if you really want to follow). We discussed a lot about the Sprints, tasks, backlogs and other things, but to make all of these concepts function properly is another story, and that is where Jira comes in. Jira is a platform suitable for agile teams, and it helps us at Outsmart keep things organized and facilitates our whole Scrum approach by supplying many types of tools. On top of that, it communicates with Bitbucket, our Git control platform, which makes things even easier by connecting our PR's with our tasks, thus helping us keeping track of everything. In this section we will explain what you can and what you should do with this tool as an Outsmart employee. The first thing you should do is make sure you have the proper access rights to the project's Jira board, contact someone in your team if you need help.

### 5.1 Backlog Screen

One of the main environments of Jira is the backlog, it holds all the project's tasks that were made at the beginning of the project, as well as all the Sprints' backlogs. You can access it through Jira's left menu, it is the first item from the top. From it you can create new Sprints and new tasks, and by clicking on a task its detail will be displayed in the right side of the screen.

[[images/Manual_De_Desenvolvimento/manual_dev_2.png]]

### 5.2 Active Sprints Screen

The other very important environment, one that you will practically live in during your Outsmart daily life, is the Active Sprints screen, which can also be accessed through the left menu, it is the second item from the top. At the top of this screen you will see the Sprint's name, and in case there is more than one active Sprint, you can filter which Sprint you want to see or even see all the sprints at once by clicking in the Switch Sprint option, which is located to the right of the Sprint's name (the Sprint's name is usually the date in which the Sprint is scheduled to terminate). In a Sprint you can see all the Sprint's tasks that were determined in your team's Sprint Planning, organized in cards on the Sprint's board allocated inside one of the Sprint's stages (which we will talk about later). By clicking on them their details will be displayed at the right side of the screen, just like in the Backlog screen. 

[[images/Manual_De_Desenvolvimento/manual_dev_3.png]]

### 5.3 Task's Details

#### 5.3.1 Task code

At the task's detail section, you will see its task code, which is the project's initials followed by a number that represents when this task was created. So for instance, in a project called SuperApp, the first task will have a task code SA-1. 

#### 5.3.2 Summary, Remaining and Estimate Time

Under the task code there is a summary of the task, followed by the estimated time needed for completing the task (determined by the PO) as well as the remaining time, which is the difference between the estimated time and the hours of work logged into this task. 

#### 5.3.3 Log Work

Next there is a log work button, which you should use to register the hours you have worked on this task (if it was assigned to you, of course). This button will only appear for you if you're the task's assignee.

#### 5.3.4 Details

Then there is a details section, where you can see details like tasks status, which is the stage in which it is in, its priority, some other details that are not important for us, and also an Epic Link that works like a tag usually for the platform it is related to (web, android or iOS). 

#### 5.3.5 People

The next part called People show who created the task and who it is assigned to, and by clicking the assign to me button you will become the assignee. You can also see the dates at which the task was created and updated. 

#### 5.3.6 Description

The task may have a description section if more information is needed in order to complete it, so you should always check this section. Also, you may leave comments at a task if you'd like, as well as send pertinent attachments when needed.

[[images/Manual_De_Desenvolvimento/manual_dev_4.png]]
[[images/Manual_De_Desenvolvimento/manual_dev_5.png]]

#### 5.3.7 Development

Also, at the Development section it will be listed all the branches and PR's that this task has been tagged (see our Gitflow Guide for more details on how this works on the Bitbucket end). If you click in one of these items it can redirect you to the corresponding Bitbucket page.

[[images/Manual_De_Desenvolvimento/manual_dev_6.png]]

#### 5.3.8 Task Stages

Now we need to talk about the task stages, which are To Do, Bugs, In Progress, PR Backlog, QA and Done. We will list those stages below, explaining the task's lifecycle.

* To Do: At the beginning of a Sprint, all the tasks that should be done are laid in the To Do section. The tasks might be already assigned to the developer's or not, depending on what was decided at the Sprint planning, and if not you should assign a task to yourself once it has been decided that you will do it. Once you decide to start working on a task you will move it's card (click on it and drag) to the In Progress stage, and take note on the time you started working on it so you can log your work hours later.

* In Progress: You have begun a task, and while you are working on it it's card should remain at the In Progress stage. When you have finished it, you should log your work by clicking on the task's card to show its details, and then clicking on log work. A window will pop up where you can type the time you have spent. Then, according to our Gitflow (see out Gitflow guide and the Git section of this guide) you will open a PR, and when this is done you will move the task's card to the PR Backlog stage.

* PR Backlog: Once you have merged your branch and closed your PR (again, if you don't understand this read our Gitflow guide), you should move the card to the QA stage. There are some cases that may be more complex, check them out below.

* QA: This represents the Quality Assurance stage. A task should be placed at this stage when the features related to it are available for the Testers to access. So, for instance, if you are working on an Android task, and you have already merged your branch, you shouldn't move it to QA unless you have made the .apk file available for testing. This prevents a situation in which the tester might see the task in the QA stage, try to run it and ends up not finding it. So be mindful when moving your cards. When the tester has confirmed that the feature is completely functional, they will move it to the Done stage. If any malfunctions are found the tester will send the card to the Bugs stage.

#### 5.3.9 Bugs

It is also part of a developer's responsibility to check if their tasks have returned to the Bugs stage. Not only that but new tasks may also be created and put directly in the Bug stage if a detected bug was not related to a particular task. To start working on a task at Bug you should do the same as if it was at the To Do stage. Once you start working on it, the task will begin its life cycle again, and it's even possible that it returns to the Bug stage.

#### 5.3.10 Done 

When the tester has finally checked that all functionalities are working like they should, they will send the task's card to the Done stage, and then the task's life has ended. When all the tasks of this Sprint are done, the Sprint may be closed by the PO.
	
## 6. DevOps

As a developer at Outsmart, you are expected to work mainly in the code related to the functionalities of the apps. However, there is much more involved in the development process that are necessary to integrate and assure that the app runs smoothly on its environment. Many of those aspects are part of the job of the so called Developer of Operations (DevOps). Naturally you may have many questions as to where your job ends and where the DevOps' begins, so do not worry. This section of the guide has the purpose of clearing up in what situations you may need the help of the DevOps. Check with your teammates who the DevOps is if you do not know.

Basically the DevOps is the one responsible for integrating the app with all the other services that are needed for the app to function. We will start with the first case where you may need their assistance, which will be at the very beginning of a project.

### 6.1 Project's Initial Requirements

When a project is about to start, that are some aspects that need to be analyzed in order to set everything you need to be able to do your job working on it. Here are a few examples:

* Storage Services: Most of our projects need a storage service, they are useful for keeping most types of files that are the app's assets, such as photos and videos for instance. While you work in the app's development you may need to use those services so they need to be set beforehand, therefore when the need for this service is identified you and your team should contact the DevOps to help set it up. At Outsmart we usually use the Amazon S3 Bucket, except in cases in which we use Firebase - a backend as a service (BaaS) - when we have Firebase Storage available. 

* Database Structure: Sometimes it's easy to use some database structure depending of the project. In this moment we try to imagine the tables and queries that the app will need based on the user's case, after that we discuss and decided what database will perform better in this situation.
Always imagine that the app could scale a lot and what are the most "heavy" queries that the app will make. We usually choose a NoSQL database as MongoDB.

### 6.2 Preparing Third Party Testing

During the development in many occasions you will need, or you will be asked to make a version of the app available for someone else to test, and initially it can be challenging. In web development you may face mostly these two distinct cases: local tests, and remote tests. Furthermore we also explain what you need to know about making mobile apps available for testing.

#### 6.2.1 Web Local Testing

This first case concerns the situation in which someone on your network wants to test the app. If you are not currently working or making any changes on your application, you may simply run your app locally (the commands for it may vary, check your project's README file) inform your IP (which you can find by typing ifconfig in a terminal window) to the third party, as well as the port in which the app is running. Let's say your IP is 192.168.100.119 and the port in which the app is running is 3000, so all the the third party needs is to type 192.168.100.119:3000 in their web browser's address bar.

Another option is to deploy the app in a local server. You may use the third party's own machine as a server, and for this they need to clone the app's repo and run it in their machine. If you wish to deploy the app to an Outsmart's dedicated servers you will need the assistance of the DevOps, so he can help you set it up by granting you access rights.

#### 6.2.2 Web Remote Testing 

Even if you have been working on the app for a while, it might not have been necessary yet to upload it to a server, but If you need to make it available for remote testing now is the time. Here at Outsmart we use the Amazon Web Services (AWS), which is a cloud platform. If this has already been done you may refer to the section App Deployment to proceed, and if not, first you will need to ask for the DevOps for help. Here is what they will need to do:

* Using the AWS console, they will set an EC2 (Elastic Cloud Computing) server using the Outsmart's AMI (Amazon Machine Image)
* Configure the security group by setting the access ports
* Clone the app's project and run the deployment commands

#### 6.2.3 Android Testing

Making an Android app available for testing is quite simple and you should not need the help of the DevOps. First you need to build an apk file using the Android Studio (simply by going to Build then Build APK), and then upload the apk file to the project's designated Google Drive's folder (if you are not sure please ask one of your coworkers).

#### 6.2.4 iOS Testing

To make your iOS app available for testing you may need to ask the DevOps to generate the certificates for you. Then you will have to generate the ipa file using the Xcode, and upload it to the project's designated Google Drive's folder (if you are not sure please ask one of your coworkers).

### 6.3 App Deployment

To deploy an app is to make it available for anyone (who have been granted the appropriate rights, of course). In this section we will guide you through this process, and explain how the DevOps should help you.

#### 6.3.1 Web Deployment

In order to deploy a Web app you usually needs to upload the static content in some hosting service, we use AWS S3 (Simple Storage Service). First ask the DevOps to configure a S3 bucket for you and give you the URL. The bucket is a "folder" that you will insert your web app files. So we use it to store and retrieve any amount of data at any time, from anywhere on the web. When you need to deploy some changes or do the first deploy the only thing you need to do is upload the new files to this S3 bucket. To optimize the time to do that we use the AWS CLI (Command Line Interface). That way you just run a terminal command.

An additional point, your browser does web caching of your site so, sometimes the changes will be in S3 but you won't see that change on browser when refresh. So do the hard refresh command cmd+shift+r. If that does work maybe the CloudFront configuration is incorrect, call your DevOps pal.

#### 6.3.2 Android Deployment

The first steps of Android Deployment are quite similar to its testing, although at every first time you do it for an app you will need the help of the DevOps. First you need to build the signed .apk file. In Android Studio go to Build, then Generate signed APK. It will ask the key store path, which if it is the first time generating the signed APK you will not have it, so you will have to create it. Check with the DevOps if it already exists and if so where to get it. It will also be needed the key store password , key alias and key password, also please check with the DevOps what those are in the case that the key store exists. In the build type section choose the "release" option, and in Signature Versions mark V1, unless the DevOps tells you otherwise. When you generate the .apk, send it to the DevOps so they can release it in the Google's Play Store.

#### 6.3.3 iOS Deployment

In order to deploy an iOS app you may upload it to the AppStore or release a TestFlight, either way you should ask the DevOps to do it for you.

### 6.4 Monitoring

To monitor everything we have two approaches: resource monitoring, and application monitoring. But what is the difference? 

The resource monitoring is the machine/server/service health check. It checks the status of the infrastructure that your application is running on. If your application is down because a natural disaster that will trigger a resource monitoring alarm, but if your application is down because of the code that will trigger a application monitoring alarm. 

The resource monitoring is up to the DevOps. He'll set the parameters and alarms at AWS Console. 
The application monitoring depends of the technology used in your application. So there are multiple options. You don't have to worry, the team will decide together.

### 6.5 Production 

In production everything is very fragile and needs a lot of attention. 

In the mobile realm the production release is made generating a .apk or .ipa file and uploading to the store. Note that if you publish an app with a bug at the store, it could take days until you could make a fix update.

In the web realm you only need to upload the files to the production bucket of your application, if you don't have a production bucket, contact your DevOps pal.