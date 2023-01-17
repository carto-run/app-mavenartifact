# app-mavenartifact

## Creating the Workload

```
tanzu apps workload create app-mavenartifact \
  --namespace dev \
  --maven-artifact app-mavenartifact \
  --maven-group com.example \
  --maven-type jar \
  --maven-version 0.0.2 \
  --image ghcr.io/carto-run/app-mavenartifact:latest \
  --label app.kubernetes.io/part-of=app-mavenartifact \
  --type web \
  --yes
```

## Logs

```
tanzu apps workload tail app-mavenartifact
```

## Configuration

| Item            | Config                                                                                |
| --------------- | ------------------------------------------------------------------------------------- |
| Scan Policy     | [default](resources/scan-policy.yaml)                                                 |
| Pipeline        | [developer-defined-tekton-pipeline](resources/developer-defined-tekton-pipeline.yaml) |
| tap-values.yaml | na                                                                                    |
| Supply Chain    | scanning-image-scan-to-url                                                            |

