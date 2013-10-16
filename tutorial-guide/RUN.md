Running Errai
=============

This section of the tutorial will show you how to deploy an Errai Application locally for development. By the end of this section you will be able to run the errai-tutorial demo in Development mode.

Prerequisites
-------------

Before attempting this you should have installed all the necessary software. For a list of required software with links and instructions, check out the [setup section](SETUP.md).

Regarding Commands
------------------

Any commands in these instructions are targetted towards users of Unix-based operating systems (namely Linux and Mac OSX). If you are a Windows user, the commands shown may need to be modified to work as described. (Please consider enhancing these instructions by submitting a [pull request](https://github.com/errai/errai-tutorial) or starting a discussion on the [forum](https://community.jboss.org/en/errai).)

Getting Started
---------------

As mentioned above, we will be working with the errai-tutorial demo. So the first thing we need to do is get a copy of the project. To clone the errai-tutorial project run

    git clone https://github.com/errai/errai-tutorial.git $HOME/errai-tutorial

You should now have a folder `errai-tutorial` in your home directory.

Maven Project Structure
-----------------------

For those new to Maven, here's a quick overview of the project structure in errai-tutorial:

* **pom.xml**

    * This XML file contains all the dependency and plugin information required for Maven to build this project.

* **src/main/java**

    * The root folder for Java source code.

* **src/main/resources**

    * The location of resources to be available to be provided to the application at run time. This folder contains configurations used by Errai.

* **src/main/webapp**

    * The location of html and css files, and any other resources to be served to clients.

Starting JBoss
--------------

To run this demo, we'll be deploying it to the JBoss Application Server. To start a standalone instance of JBoss, run

    $JBOSS_HOME/standalone/bin/standalone.sh

where `$JBOSS_HOME` is the path to JBoss folder you unzipped from the [setup](SETUP.md) instructions.

Now go to http://localhost:8080 in your web browser. If the server is running correctly, you should see a page saying "Your JBoss Application Server 7 is running."

Building and Deploying
----------------------

It's time to really get things started. While the JBoss server is still running, run the command

    mvn clean package jboss-as:deploy

**Note**: This command may take a while the first time it is executed. This is because Maven must fetch all the dependencies required to compile the project. This is only necessary the first time you build, making subsequent builds much faster.

### For Other Application Servers

If you are using a different Application Server you should be able to manually deploy the demo by running

    mvn clean package

and then copying `errai-tutorial/target/errai-tutorial.war` to the proper deployments folder.

Verify Deployment
-----------------

Once Maven is done building and deploying the demo, verify that it is running at http://localhost:8080/errai-tutorial . This demo features a simple complaint form application. Users can submit complaints from the main page, and view submitted complaints from an administrative page (linked at the bottom of the home page).

To really see the demo in action, you should run two test it with at least two browser windows open. Try submitting complaints with one window and see them live update to the admin screen. Or open two admin windows and completing complaints.

**Note**: By default, JBoss will only serve the application locally. If you would like to try the demo on mutliple devices, you can configure JBoss to bind to all addresses by editing `$JBOSS_HOME/standalone/configuration/standalone.xml`. Just these lines:

```
<interface name="public">
   <inet-address value="${jboss.bind.address.public:127.0.0.1}"/>
</interface>
```

And replace them with this:

```
<interface name="public">
    <any-address/>
</interface>
```

Run Development Mode
--------------------

Development mode allows us to see the results of changes in our code without having to recompile the whole app. It will also allow us to debug the Java source code that is being run in the browser.

To run the demo in development mode, run the following command **while JBoss is running with errai-tutorial deployed**:

    mvn gwt:run

When the *GWT Development Mode* window appears, click "Launch Default Browser".

Debugging with Development Mode and Eclipse
-------------------------------------------

To debug client-side code, you can simply run:

    mvn gwt:debug

and attach your IDE's remote debugger. Below are more detailed instructions on how to set this up with Eclipse (Kepler).

### Importing the Project to Eclipse

First we'll need to get the project into Eclipse:

* In the menu, go to "File" > "Import..."

* In the popup window, select "Maven" > "Existing Maven Projects" and click "Next"

* Click "Browse", find and select the errai-tutorial folder

* Check that there is only one checkbox listed under "Projects" and that it is selected

* Click "Finished"

### Creating a Remote Debug Configuration

Now that we have a project setup, we can make a remote debug configuration:

* In the menu, go to "Run" > "Debug Configurations..."

* Find and select "Remote Java Application" in the left pane and click "New Launch Configuration" in the top left corner

* Under "Project" click "Browse" and select the errai-tutorial project.

* Name the configuration, click "Close", and save the changes when prompted

You will now be able run this configuration to debug client-side code after running `mvn gwt:debug` in the command line.