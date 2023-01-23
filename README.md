# app-mavenartifact

## Creating the Workload

```
tanzu apps workload create app-mavenartifact \
  --namespace dev \
  --maven-artifact app-mavenartifact \
  --maven-group com.example \
  --maven-type jar \
  --maven-version RELEASE \
  --label apps.tanzu.vmware.com/has-tests=true \
  --label app.kubernetes.io/part-of=app-mavenartifact \
  --param-yaml testing_pipeline_matching_labels='{"apps.tanzu.vmware.com/pipeline":"noop-pipeline"}' \
  --param-yaml testing_pipeline_params='{}' \
  --type web \
  --yes
```

### Simulating a broken source-provider

```
tanzu apps workload create app-mavenartifact-fails-at-source-provider \
  --namespace dev \
  --maven-artifact app-mavenartifact-fails-at-source-provider \
  --maven-group com.example \
  --maven-type war \
  --maven-version RELEASE \
  --label apps.tanzu.vmware.com/has-tests=true \
  --label app.kubernetes.io/part-of=app-mavenartifact-fails-at-source-provider \
  --param-yaml testing_pipeline_matching_labels='{"apps.tanzu.vmware.com/pipeline":"noop-pipeline"}' \
  --param-yaml testing_pipeline_params='{}' \
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

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  labels:
    apps.tanzu.vmware.com/pipeline: noop-pipeline
  name: developer-defined-noop-tekton-pipeline
  namespace: dev
spec:
  params:
  - name: source-url
    type: string
  - name: source-revision
    type: string
  tasks:
  - name: noop-task
    params:
    - name: source-url
      value: $(params.source-url)
    - name: source-revision
      value: $(params.source-revision)
    taskSpec:
      metadata: {}
      params:
      - name: source-url
        type: string
      - name: source-revision
        type: string
      steps:
      - image: bash
        name: noop
        resources: {}
        script: |
          echo "Nothing to do for $(params.source-url)/$(params.source-revision)"
```  

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
<td> source-test-scan-to-url </td>
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
