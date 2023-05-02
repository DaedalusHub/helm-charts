# Overview
Helm charts for Daedalus AI projects.

## DaedalusAI Installation
Add the helm repo for daedalushub:
```bash
helm repo add daedalushub https://daedalushub.github.io/helm-charts/
```

Create a `values.yaml` file for the DaedalusAI chart and customize as needed:
```bash
cat <<EOF > values.yaml
deployment:
  image: 557634078649.dkr.ecr.us-west-2.amazonaws.com/daedalushub/daedalus-ai
  env:
    threads: 14
    contextSize: 512
    modelsPath: "/models"
# Optionally create a PVC, mount the PV to the LocalAI Deployment,
# and download a model to prepopulate the models directory
modelsVolume:
  enabled: true
  url: "https://gpt4all.io/models/ggml-gpt4all-j.bin"
  pvc:
    size: 6Gi
    accessModes:
    - ReadWriteOnce
  auth:
    # Optional value for HTTP basic access authentication header
    basic: "" # 'username:password' base64 encoded
service:
  type: LoadBalancer
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "1200"
```
Install the DaedalusAI chart:
```bash
helm install daedalus-ai DaedalusHub/daedalus-ai -f values.yaml
```
