# Counterintutitive errors when using Helm

## Error

```
Error: INSTALLATION FAILED: template: xxx/templates/_helpers.tpl:6:18: executing "foo.bar" at <.Chart.Name>: nil pointer evaluating interface {}.Name
```

This error usually occurs when you execute `{{ template "foo.bar" . }}` inside a `range` block

### Solution

Save `.` outside of the `range` block
 
```mustache
{{- $root := . -}}
{{- range $volume := .Values.volumes }}
   - {{ template "foo.bar" $root }} # use $root instead of `.`
...
```
