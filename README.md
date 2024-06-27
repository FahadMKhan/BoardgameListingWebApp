**Board Game Database Full-Stack Web Application.**

https://www.youtube.com/watch?v=NnkUGzaqqOc

![image](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/7ac1b768-61e2-4bc9-92f8-926f0564e977)

**Description**
Board Game Database Full-Stack Web Application. This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.

**Technologies**
Java
Spring Boot
Amazon Web Services(AWS) EC2
Thymeleaf
Thymeleaf Fragments
HTML5
CSS
JavaScript
Spring MVC
JDBC
H2 Database Engine (In-memory)
JUnit test framework
Spring Security
Twitter Bootstrap
Maven

**Features**
Full-Stack Application
UI components created with Thymeleaf and styled with Twitter Bootstrap
Authentication and authorization using Spring Security
Authentication by allowing the users to authenticate with a username and password
Authorization by granting different permissions based on the roles (non-members, users, and managers)
Different roles (non-members, users, and managers) with varying levels of permissions
Non-members only can see the boardgame lists and reviews
Users can add board games and write reviews
Managers can edit and delete the reviews
Deployed the application on AWS EC2
JUnit test framework for unit testing
Spring MVC best practices to segregate views, controllers, and database packages
JDBC for database connectivity and interaction
CRUD (Create, Read, Update, Delete) operations for managing data in the database
Schema.sql file to customize the schema and input initial data
Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

**How to Run**
Clone the repository
Open the project in your IDE of choice
Run the application
To use initial user data, use the following credentials.
username: bugs | password: bunny (user role)
username: daffy | password: duck (manager role)
You can also sign-up as a new user and customize your role to play with the application! 😊

=======================================================================================================================

**The Ultimate CICD Corporate DevOps Pipeline Project | Real-Time DevOps Project:**


**The task will be divided into 4 phases:**

**-- Phase 1:**

* We will create a Network Environment.
The reason for a Network Environment is to that the resources and applications that we will be working with will be "Private". They will be in an isolated environment so no outside entity can access them. Lastly we will make sure that the deployment is secure.

* We will be setting up K8s cluster our any other cluster to deploy our application. Once the K8s cluster has been deployed, we will be scanning the cluster for any vulnerabilities or issues and for that we will be using a security tool for this.

*  We will be creating multiple VMs in a secure isolated environment and on these VM's we will be setting up different servers i.e. SonarQube, Nexus and tools i.e. Jenkins, Monitoring tools which will be used for monitoring the applications that we will be using to deploy.

**--Phase 2:**

* We will be creating a Git-Repo and this Git-Repo should be "Private" so no outside entity can access it.

* After creating the git-repo, we will be pushing our source code to it.

* After the SC has been pushed, we will make sure that it is visible to us.

**-- Phase 3:**

 * In this phase we will start working with CICD pipeline. While doing this we have to make sure that we are following the "best practices" and we are taking "security measures".

* We will deploy our application.

* We will configure mail notification.

**-- Phase 4:**

* In this phase we will be setting up the monitoring tools to monitor our application. The monitoring will be done on two levels, "system level" and "website level". On system level we can monitor things like CPU, RAM etc. On website level we will be monitoring traffic etc.

-------------------------------------------------------------------------------------------
Here are some of the required png's:

![Nexus-setting-1](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/711c4dcb-bff2-46b7-8deb-26403da9b820)


![Nexus-setting-2](https://github.com/FahadMKhan/BoardgameListingWebApp/assets/97802721/fcea7679-bf72-4b79-8bb7-8a4eea43906c)
