# Counterintutitive errors when using Helm

## Error

```sh
Error: INSTALLATION FAILED: template: xxx/templates/_helpers.tpl:6:18: executing "foo.bar" at <.Chart.Name>: nil pointer evaluating interface {}.Name

# or

Error: INSTALLATION FAILED: template: xxx/templates/_helpers.tpl:6:45: executing "foo.bar" at <63>: invalid value; expected string
```

This error usually occurs when you execute `{{ template "foo.bar" . }}` inside a `range` block

### Solution

Save `.` outside of the `range` block
 
```diff
+ {{- $root := . -}}
{{- range $volume := .Values.volumes }}
-   - {{ template "foo.bar" . }}
+   - {{ template "foo.bar" $root }} # use $root instead of `.`
...
```

## Error

```sh
Error: INSTALLATION FAILED: YAML parse error on xx/deployment.yaml: error converting YAML to JSON: yaml: line 24: mapping values are not allowed in this context
```

This error usually occurs when you improperly use the `-` (dash) symbol in the go template, esp after several kinds of statements, such as `if`, `range`, etc.

### Solution

```diff
- {{- if .Values.tls -}}
+ {{- if .Values.tls }}
  tls: true
{{- end -}}
```
