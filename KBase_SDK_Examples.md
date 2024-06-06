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
      *  "url": "",
      *  "name":"{username}ContigFilter",
      *  "method": "run_{username}ContigFilter_max"
 *   }
*  Next, add max_length to the input_mapping
*  {
      *  "input_parameter": "min_length",
      *  "target_property": "min_length"
 * },
 * {
      *  "input_parameter": "max_length",
      *  "target_property": "max_length"
 * }
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
    * ui-name: |
    *    Maximum contig length
   * short-hint: |
     *   Maximum required length of every contig in the assembly
* Finally, run kb-sdk validate again and it should pass!


# Implement Code
* The actual code for your app lives in the Python package under lib/{username}ContigFilter
* The entry point, where your code is initially called, lives in lib/{username}ContigFilter/{username}ContigFilterImpl.py. This is where you edit your own Python code
* In a real-world app, you may want to split up your code into several Python modules and packages.
  
## Receive Parameters
* Open method_nameImpl.py and find the run_{username}ContigFilter_max method, which should have some auto-generated boilerplate code and docstrings. Edit code between the comments #BEGIN run_{username}ContigFilter_max and #END run_{username}ContigFilter_max
* Betweem the comments, add a print statement to let us see what's getting passed into our method
* EX: print(params['min_length'], params['max_length'], params['assembly_ref'])

## Initialize a Test
* Python automatically runs all methods that start with the name test, so temporarily rename all tests for the old app so they don't run until we are done working on the new app
  * EX: change def test_run_{username}ContigFilter_ok(self) to def my_test_run_{username}ContigFilter_ok(self)
* Now add your own test for the new app method at the bottom of the test class and call it the test_run_{username}ConfigFilter_max(self)
* We need to provide three parameters to our function
  1. Workspace Name
  2. Assembly Reference String
  3. Min length integer
 * Run kb-sdk test and if everything works, the docker container will boot up, the run_{username}ContigFilter_max method will be called and output will print

## Set the Callback URL and Scratch Path
* The callback URL points to a server that is used to spin up other SDK apps that we'll need to use in our own app.
* EX: When we use AssemblyUtil, our app makes a request to the callback server, which spins up a separate docker container that runs AssemblyUtil.
* Scratch is a special directory that we can use to store files used to run an app. The directory is also accessible by other apps, like Assembly Util.
* Remember, Scratch is temporary and its files vanish when your app stops running. To generate persistent data, use Reports
* This code was added to the _init__ method to enable callbacks/scratch directory
* #BEGIN_CONSTRUCTOR
* self.callback_url = os.environ['SDK_CALLBACK_URL']
* self.shared_folder = config['scratch']
* #END_CONSTRUCTOR
* AssemblyUtil can convert input data to a FASTA file that the app can access
* This code can be used to install AssemblyUtil
   * kb-sdk install AssemblyUtil
 * Don't forget to git add new dependencies to your source control when you run kb-sdk install
 * In your {username}ContigFilterImpl.py file, import the module with:
 * from installed_clients.AssemblyUtilClient import AssemblyUtil

## Add Some Basic Validations
