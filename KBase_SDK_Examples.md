# Hello World Example
* First define important bash variables;
* your_kbase_username=jane.smith
* username=${your_kbase_username
* module_name=HelloWorld
* kb-sdk init -- language python --user $ {your_kbase_username} $ {username}$ {module_name}
* cd $ {username} $ {module_name}
* make

## Code Implementation
* The actual code lives in the Python package under lib/{username}HelloWorld
* The entrypoint lives in the file language python --user $ {your_kbase_username} 
 $ {username}${module_name}
* To separate auto generated code from developer code, developer code belongs between sets of #BEGIN and #END comments
* #BEGIN_CONSTRUCTOR
* #END_CONSTRUCTOR

## Run First Test
* As a default, your {username}HelloWorldImpl.py file is tested using test/{username}HelloWorld_server_test.py
* Python will automatically run all methods that start with the name test
* At the bottom of the test file, find the method test_your_method. The default test is to call run_{username}HelloWorld with a workpsace_name for the test and parameter_1 of "Hello World".
* Add a simple print statement to the end of the test method
* print("report_name", ret[0]["report_name"])
* Make sure you put your developer token in test_local/test.cfg
* Run kb-sdk test and if everything works you'll see the docker container boot up, the run_{username}HelloWorld method will get called, and you'll see printed output, which includes these two lines:
* Input parameter Hello World!
* report_name report_675e061a-2fce-47aa-ac85-67e3ec975776



# ContigFilter Example
* The ContigFilter example adds more features of the SDK (since in practice, apps must interact with the workspace/narrative, database objects, and create reports from output)
  
## Step 1: The App Design Checklist
* 
