# Creating Your Own Module
## Getting the Template
* Start with this template module: https://github.com/kbase-sfa-2021/example_sdk_app
* Click "Use this Template", then "Create a New Repository"
* The name for your repository should be a valid Python module identifier (lowercase, underscores, brief as possible)
* Clone the repository
## Finding the Report
* Open the cloned repository in your desired code editor (Vim, VS Code, etc)
* Switch to the repository's directory
* Make sure you've added your authentication token to test_local/test.cfg
* Run kb-sdk test to generate the sample report
* You can find your report in your test local folder
  *  In the test local folder, navigate to workdir
  *  From workdir, navigate to tmp
  *  In tmp is the reports folder
  *  Finally, the reports folder contains the index.html file for your report
* The report can be viewed using SOCKS Proxy

## Making Modifications
*  The following command will allow you to edit the index.html file directly using Vim
```
vim test_local/workdir/tmp/reports/index.html
```
* You can also edit the report template, and after running kb-sdk test, the modifications will carry over to your report.
```
vim lib/templates/report.html
```
* 
