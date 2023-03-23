This is a demonstration how you can share values between multiple helm Subcharts using the `global` key and using the template syntax as value in the values.yaml file.

## Usage

To test the output use this command:

```bash
helm install --dry-run demo my-chart
```

Each Subchart is a standalone chart and can be used separately.
To test one Subchart use this command:

```bash
  helm install --dry-run demo my-chart/charts/subchart-main
```


## Output

As you can see the values are shared between the Subcharts and the parent chart.

```yaml
# Source: mychart/charts/subchart-main/templates/subchart-main.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: demo-cfg-subchart-main
data:
  # Important: The values containing the template syntax. Therefore the template function is required to get the defined values in the global section.
  name: my-service-1 SHARED
  endpoint: my-service-1-endpoint SHARED
---
# Source: mychart/charts/subchart01/templates/subchart01-cfg.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: demo-cfg-subchart01
data:
  name: my-service-1 SHARED
  endpoint: my-service-1-endpoint SHARED
---
# Source: mychart/templates/parent.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: demo-cfg-parent
data:
  my-value: my-service-1 SHARED
```