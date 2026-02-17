# Example folder for custom templates

This is a simple example folder to get started with custom bundle templates in the workspace. 
The example template demonstrates conditional logic to include or exclude resources based on user input. 

## Folder for Custom Templates Structure
To use [custom templates in the workspace](https://docs.google.com/document/d/1b1NAxIMNhL5wiFCo95qqKm5q8GN8AvTg6tpKyuqZ7Hg/edit?tab=t.0#heading=h.xmytrhd54qi0), store all templates that you want to use in a single folder/repo: 
```
folder-for-custom-templates/
├── example custom template
├── custom template 2
├── custom template 3
├── ...         
```
Note that the name of each template is derived from the respective folder name. 

## Template Structure
```
example custom template/
├── databricks_template_schema.json                  # Defines template inputs
└── template/
    ├── create_files.tmpl                            # Create files based on user input
    └── {{.project_name}}/                           # Templated project directory
        ├── databricks.yml.tmpl                      # Bundle configuration with conditional logic
        ├── resources/
        │   ├── sample_job.job.yml.tmpl              # Job configuration (uses {{.project_name}})
        │   └── sample_pipeline.pipeline.yml.tmpl    # Pipeline configuration (uses {{.project_name}})
        └── src/
            ├── hello_world.ipynb                    
            └── {{.project_name}}_pipeline.ipynb.tmpl 
```

## What This Template Includes

This template allows you to optionally create a:

1. Job with a notebook task 
2. Spark Declarative Pipeline 

If you select "no" for either option during initialization, those resources will not be created during bundle initialization.

## How Conditional File Generation Works

The `create_files.tmpl` is processed first during bundle initialization and uses the `skip` function to conditionally exclude files from generation based on user input:

```go
{{- if ne .include_hello_world_job "yes"}}
  {{skip "{{.project_name}}/src/hello_world.ipynb"}}
  {{skip "{{.project_name}}/resources/sample_job.job.yml"}}
{{- end}}

{{- if ne .include_dlt_pipeline "yes"}}
  {{skip "{{.project_name}}/src/{{.project_name}}_pipeline.ipynb"}}
  {{skip "{{.project_name}}/resources/sample_pipeline.pipeline.yml"}}
{{- end}}
```

Files only need the `.tmpl` extension if they use Go template syntax within them:
- Files containing template variables (e.g., `{{.project_name}}`)
- Files with conditional logic (e.g., `{{if}}` statements)

The `databricks.yml.tmpl` also uses Go template syntax to conditionally include resource files in the bundle to ensure the `include:` section only appears when at least one resource is selected:

```yaml
{{- if or (eq .include_hello_world_job "yes") (eq .include_dlt_pipeline "yes")}}
include:
{{- if eq .include_hello_world_job "yes"}}
  - resources/sample_job.job.yml
{{- end}}
{{- if eq .include_dlt_pipeline "yes"}}
  - resources/sample_pipeline.pipeline.yml
{{- end}}
{{- end}}
```

## Learn More

- [Custom Template Documentation](https://docs.databricks.com/dev-tools/bundles/templates.html)
- [Go Template Syntax](https://pkg.go.dev/text/template)
