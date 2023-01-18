# app-mavenartifact

## Creating the Workload

```
tanzu apps workload create app-mavenartifact \
  --namespace dev \
  --maven-artifact app-mavenartifact \
  --maven-group com.example \
  --maven-type jar \
  --maven-version 0.0.2 \
  --label apps.tanzu.vmware.com/has-tests=true \
  --label app.kubernetes.io/part-of=app-mavenartifact \
  --type web \
  --yes
```

## Logs

```
tanzu apps workload tail app-mavenartifact
```

## Configuration

<table>

<tr>
<th> Item </th>
<th> Config </th>
</tr>

<tr>
<td> Scan Policy </td>
<td> 
  
[default](resources/scan-policy.yaml)
  
</td>
</tr>

<tr>
<td> Pipeline </td>
<td>
  
[developer-defined-tekton-pipeline](resources/developer-defined-tekton-pipeline.yaml)
  
</td>
</tr>

<tr>
<td> tap-values.yaml </td>
<td> 

```yaml
maven:
  repository:
    url: https://us-east1-maven.pkg.dev/ship-interfaces-dev/test-repository
    secret_name: maven-credentials
```

</td>
</tr>

<tr>
<td> Supply Chain </td>
<td> scanning-image-scan-to-url </td>
</tr>

<tr>
<td> Maven Credentials </td>
<td>

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: maven-credentials
  namespace: dev
type: Opaque
stringData:
  username: _json_key_base64
  password: |
    <gcp service account key>
```

</td>
</tr>

</table>
