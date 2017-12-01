# Refine API
This is a generic API reference for interacting with Refine's HTTP API.

## Create project:

> **Command:** _POST /command/core/create-project-from-upload_

When uploading files you will need to send the data as `multipart/form-data`. This is different to all other API calls which use a mixture of query string and POST parameters.

multipart form-data:

      'project-file' : file contents
      'project-name' : project name
      'format' : format of data in project-file (e.g. 'text/line-based/*sv') [optional]
      'options' : json object containing options relevant to the file format [optional]

The formats supported will depend on the version of OpenRefine you are using and any Extensions you have installed. The common formats include:

* 'text/line-based': Line-based text files
* 'text/line-based/*sv': CSV / TSV / separator-based files [separator to be used in specified in the json submitted to the options parameter]
* 'text/line-based/fixed-width': Fixed-width field text files
* 'binary/text/xml/xls/xlsx': Excel files
* 'text/json': JSON files
* 'text/xml': XML files

If the format is omitted OpenRefine will try to guess the format based on the file extension and MIME type.
The values which can be specified in the JSON object submitted to the 'options' parameter will vary depending on the format being imported. If not specified the options will either be guessed at by OpenRefine (e.g. separator being used in a separated values file) or a default value used. The import options for each file format are not currently documented, but can be seen in the OpenRefine GUI interface when importing a file of the relevant format.

If the project creation is successful, you will be redirected to a URL of the form:

      http://127.0.0.1:3333/project?project=<project id>

From the project parameter you can extract the project id for use in future API calls. The content of the response is the HTML for the OpenRefine interface for viewing the project.

## Apply operations

> **Command:** _POST /command/core/apply-operations?_

      'project' : project id
      'operations' : valid json array of OpenRefine operations

On success returns JSON response
`{ "code" : "ok" }`
    
## Export rows

> **Command:** _POST /command/core/export-rows_

      'project' : project id
      'engine' : JSON string... (e.g. '{"facets":[],"mode":"row-based"}')
      'format' : format... (e.g 'tsv', 'csv')
      
Returns exported row data in the specified format. The formats supported will depend on the version of OpenRefine you are using and any Extensions you have installed. The common formats include:

* csv
* tsv
* xls
* xlsx
* ods
* html
    
## Delete project

> **Command:** _POST /command/core/delete-project_

      'project' : project id...
      
Returns JSON response
    
## Check status of async processes

> **Command:** _POST /command/core/get-processes_

      'project' : project id...
      
Returns JSON response

## Get all project metadata:
    
> **Command:** _GET /command/core/get-all-project-metadata_

Recovers the meta data for all projects. This includes the project's id, name, time of creation and last time of modification.
    
### Response:
```
{
    "projects":{
        "[project_id]":{
            "name":"[project_name]",
            "created":"[project_creation_time]",
            "modified":"[project_modification_time]"
        },
        ...[More projects]...
    }
}
```
    
## Expression Preview
> **Command:** _POST /command/core/preview-expression_

Pass some expression (GREL or otherwise) to the server where it will be executed on selected columns and the result returned.

### Parameters:
* **cellIndex**: _[column]_  
The cell/column you wish to execute the expression on.
* **expression**: _[language]_:_[expression]_  
The expression to execute. The language can either be grel, jython or clojure. Example: grel:value.toLowercase()
* **project**: _[project_id]_  
The project id to execute the expression on.
* **repeat**: _[repeat]_  
A boolean value (true/false) indicating whether or not this command should be repeated multiple times. A repeated command will be executed until the result of the current iteration equals the result of the previous iteration.
* **repeatCount**: _[repeatCount]_  
The maximum amount of times a command will be repeated.

### Response:
**On success:**
```
{
  "code": "ok",
  "results" : [result_array]
}
```

The result array will hold up to ten results, depending on how many rows there are in the project that was specified by the [project_id] parameter. Each result is the string that would be put in the cell if the GREL command was executed on that cell. Note that any expression that would return an array or JSon object will be jsonized, although the output can differ slightly from the jsonize() function.

**On error:**
```JSON
{
  "code": "error",
  "type": "[error_type]",
  "message": "[error message]"
}
```