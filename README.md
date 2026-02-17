# Example folder for custom templates

This is a simple example folder to get started with custom bundle templates in the workspace. 
The example template demonstrates conditional logic to include or exclude resources based on user input. 

## What This Template Includes

This template allows you to optionally create a:

1. Job with a notebook task 
2. Spark Declarative Pipeline 

If you select "no" for either option during initialization, those resources will not be created during bundle initialization.

## Folder for Custom Templates Structure
To use [custom templates in the workspace](https://docs.google.com/document/d/1b1NAxIMNhL5wiFCo95qqKm5q8GN8AvTg6tpKyuqZ7Hg/edit?tab=t.0#heading=h.xmytrhd54qi0), store all templates that you want to use in a single folder/repo: 
```
folder-for-custom-templates/
├── example-custom-template 
├── custom-template-2
├── custom-template-3
├── ...         
```
Note that the name of each template is derived from the respective folder name. 

## Template Structure

```
example-custom-template/
├── databricks_template_schema.json                  # Defines template inputs
├── README.md                                        # This file
└── template/
    └── {{.project_name}}/                           # Templated project directory
        ├── databricks.yml.tmpl                      # Bundle configuration with conditional logic
        └── resources/
            ├── sample_job.job.yml.tmpl              # YAML with job configuration 
            └── sample_pipeline.pipeline.yml.tmpl    # YAML with pipeline configuration 
        └── src/
            ├── hello_world_notebook.ipynb    
            └── {{.project_name}}_pipeline.ipynb            
```

## Go template syntax

The `databricks.yml.tmpl` uses Go template syntax to conditionally include resources:

```yaml
{{if eq .include_hello_world_job "yes"}}
  jobs:
    hello_world_job:
      # Job configuration...
{{end}}
```

## Learn More

- [Custom Template Documentation](https://docs.databricks.com/dev-tools/bundles/templates.html)
- [Go Template Syntax](https://pkg.go.dev/text/template)
