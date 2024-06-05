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
* Does my App depend on any compiled or third-party packages? If so, are they licensed with a KBase compliant license? If so, is there a version that's executable by one of the SDK languages or as a linux binary?
* What are the CPU and RAM requirements for my app?
* Does my App require reference/precomputed data?
* Does my App operate on KBase datatypes? If so, do I need a FileUtil to convert the Workspace object to a desired file format?
* What parameters do I want users to be able to provide (EX: min_length, max_length)
* Does my App produce any KBase DataTypes? If so, do I need a FileUtil to save the desired file format as a workspace object?
* How do I want to display results to users?

  ## Step 2: Module Description
  * Your username is added to make sure your module has a unique name, and name the module itself something like ContigFilter
  * To get started, run the following code
  * kb-sdk init --example --language python --user ${your_kbase_username} ${username}${module_name}
  * cd ${username}${module_name}
  * make
 
  ## Step 3: The Specification File
  * In the root directory of the module, the specification file ({username}ContigFilter.spec) uses KIDL to specify which functions will be accessible to other SDK services, as well as to specify input and output types. The specification file generates client code in any KBase supported language
  * There are two types of specifications...
    1. Input/Output Structures
    typedef structure{
      string report_name;
      string report_ref; (indicate which report object should be displayed)
    } ReportResults;
    2. Functions
      Defines the functions that may be called by other SDK modules or app cells
 * When editing the spec file, copy the funcdef line and give the function a unique name
 * funcdef run_{username}ContigFilter(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;
 * After these steps, return to the module's root directory and run make. Remember, you must rerun make after each change to the KIDL specification to regenerate implementation and server code used in the codebase

# Narrative User Interface
* The user interface (UI) that users see in narratives and in the app catalog are defined in two files: spec.json and display.yaml, and they're in the directory called ui/narrative/methods/run_{username}{module_name}

## Update spec.json
* Copy the directory named ui/narrative/methods/run_{username}ContigFilter and create a directory for the new app...
* $ cd ui/narrative/methods/
* $ cp -r run_${username}ContigFilter run_${username}ContigFilter_max
* When you open up ui/narrative/methods/run_{username}ContigFilter_max/spec.json, you see two input parameters, which will generate UI form elements in the narrative that allow the user to input data into your app.
* Each parameter object has a number of options:
  1. We want both parameters to be required ("optional": false)
  2. We want the assembly_ref to be a reference to either an Assembly or ContigSet object
  3. We want the min_length parameter to be validated as an integer, and we don't want to allow negative numbers (minimum valid integer to be 0)
  4. Add the other input parameter max_length with similar values
     
 * In the section under behavior, change run_{username}ContigFilter to run_{username}ContigFilter_max (name is the name of the module, method is the name of the app)
 *  "service-mapping": {
        "url": "",
        "name":"{username}ContigFilter",
        "method": "run_{username}ContigFilter_max"
    }
*  Next, add max_length to the input_mapping
*  {
        "input_parameter": "min_length",
        "target_property": "min_length"
    },
    {
        "input_parameter": "max_length",
        "target_property": "max_length"
    }
* Add commas after the min_length parameters to maintain valid JSON syntax
* When you make changes to UI files, validate the syntax of your changes by running:
* $ kb-sdk validate

## Update display.yaml
* The text written in ui/narrative/methods/run_{username}ContigFilter/display.yaml holds text content for your app. The text written here will show up in the narrative and in the App Catalog for each form element. You only need to set this text for parameters that actually display in the form.
* Open display.yaml and update its name and tooltip to say something related to filtering assembly files based on contig length with both a min and a max filter.
* The parameter entry for "min_length" looks like this
* min_length:
        ui-name: |
            Min Length Threshold
        short-hint: |
            All contigs below this length will be removed
* Edit the file and add the max_length parameter, which looks like this...
* max_length:
    ui-name: |
        Maximum contig length
    short-hint: |
        Maximum required length of every contig in the assembly
* Finally, run kb-sdk validate again and it should pass!