# Simple Databricks Asset Bundle Template

This is a simple example template for Databricks Asset Bundles (DAB) that demonstrates condiitonal logic to include or exclude resources based on user input.

## What This Template Includes

This template allows you to optionally create:

1. **Hello World Job** 
2. **Spark Declarative Pipeline** 

If you select "no" for either option during initialization, those resources will not be created in your bundle.

## Template Structure

```
example-custom-template/
├── databricks_template_schema.json    # Defines template inputs
├── README.md                          # This file
└── template/
    └── {{.project_name}}/            # Templated project directory
        ├── databricks.yml.tmpl        # Bundle configuration with conditional logic
        └── src/
            ├── hello_world_notebook.ipynb    # Simple hello world notebook
            └── dlt_pipeline.ipynb            # Simple DLT pipeline notebook
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
