# Example Databricks Custom Bundle Template

This is a simple example template for Databricks Asset Bundles (DAB) that demonstrates condiitonal logic to include or exclude resources based on user input.

## What This Template Includes

This template allows you to optionally create a:

1. Job with a notebook task 
2. Spark Declarative Pipeline 

If you select "no" for either option during initialization, those resources will not be created during bundle initialization.

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

## Folder for Custom Templates Structure
To use [custom templates in the workspace](https://docs.google.com/document/d/1b1NAxIMNhL5wiFCo95qqKm5q8GN8AvTg6tpKyuqZ7Hg/edit?tab=t.0#heading=h.xmytrhd54qi0), store all templates that you want to use in a single folder/repo: 
```
folder-for-custom-templates/
├── custom-template-1    
├── custom-template-2
├── custom-template-3
├── ...         
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
