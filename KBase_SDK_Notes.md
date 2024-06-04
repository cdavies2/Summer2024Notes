# Concepts and Overview
## The Basics

Apps on Kbase are created using the KBase SDK. When you run an app in a narrative, it runs in a docker container on KBase's servers, which allows you to package and compile programs and dependencies and run them anywhere.
You, as the SDK developer, define your input parameter types, packages and programs to run, and output types, text, and HTML to show the user when the app finishes

## The Workflow
1. User Input-users have a set of form elements that can be customized to accept many kinds of data. For parameters that refer to larger data sets, they're passed into your app as references, or URLs used to download the actual files
2. App Code and Data Management-input is converted into a Python dictionary, validation checks are performed, and the dictionary is passed to a method
3. Output and Reporting-when an app finishes producing data, it typically forms a KBaseReport object that could contain an HTML page showing results, links to files (PDFs, CSVs) that your app produced, or simple text messages about the results.
   
## Command Line Interface
kb-sdk is provided to make it easy to initialize, validate, compile, and test your app

## Composing Apps
You can use other KBase apps as dependencies. When you run another app within yours, it runs in its own, separate docker container.

## KBase Types and Parameters
All data on KBase is typed using an interface definition language called KIDL, allowing Kbase to easily cross-reference data, make inferences, and check app-data compatibility

# Working with Files and Data
## Scratch
Scratch is a special location to store files that all containers have access to. However, Scratch is ephemeral, meaning any files in Scratch are gone when your app stops running
## The Workspace
The Workspace refers to file storage servers used by KBase. Use the workspace for datasets that you want to be available to users.
Datasets stored in the workspace are called "objects"
1. Every object conforms to a type specification (String, integer, float, list, structure, tuple)
2. Objects are versioned (V1, V2)
3. The "reference" for an object has the format workspace_id/object_id/version
4. Workspaces are authenticated
## The Catalog
Catalogs are registries of KBase apps, and there are three separate types
1. dev-prototype and tweak your app within the narrative
2. beta-the app is ready for release but requires testing
3. release-the app is visible to normal KBase users
When you register your new app, it is available in the dev catalog

# Docker
KBase module code is run using Docker, which allows you to easily install required system tools and dependencies, and to test your build on your own computer before registering the model with KBase
# Installation
1. Once docker is installed, pull down the KBase SDK Image
$ docker pull kbase/kb-sdk
2. Add the kb-sdk as a global command by linking it in your $PATH. Place the script in a directory like ~/bin:
$ mkdir $HOME/bin/
#Generate the kb-sdk script and put it in ~/bin/kb-sdk
$ docker run kbase/kb-sdk genscript > $HOME/bin/kb-sdk
$ chmod +x $HOME/bin/kb-sdk
#Add ~/bin to your $PATH if it is not already there
$ export PATH=$PATH:$HOME/bin/
#You might want to put the above command in your ~/.bashrc or ~/.zshrc:
$ echo "export PATH=\$PATH:$HOME/bin/" >> ~/.bashrc
3.  Test the installation by running the kb-sdk help command
$ kb-sdk help
4.   List the kb-sdk version to ensure that the latest image is used
$ kb-sdk version

# Initialize the Module
KBase SDK can generate most of the required componenets of a new module. The basic options of the command are...
$ kb-sdk init [--example] [--verbose] [--language language] [--user your_kbase_username] module_name
1. module_name must be unique across all registered SDK modules in KBase
2. You'll use the variables your_kbase_username and username multiple times so for convenience, define them (your_kbase_username=jane.smith) and username=${your_kbase_username}
3. module_name will change based on example, but it can first be defined as module_name=HelloWorld

## Bootstrapping the HelloWorld Module
$ kb-sdk init --language python --user ${your_kbase_username} ${username}${module_name}
1. This creates a directory called {username}HelloWorld (with the username making your copy unique)
2. Include the -u or


