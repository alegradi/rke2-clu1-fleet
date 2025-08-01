# rke2-clu1

This repo contains the configuration of deployments on `rke2-clu1`. This is being used by Fleet in Rancher.

## How to use

1. Have your Kubernetes cluster added to Rancher. In this case `rke2-clu1`
2. Generate an ssh-keypair that you will use to allow Fleet to watch for changes in your repo, pull it and apply to your cluster.
```bash
ssh-keygen -t rsa -b 4096 -C "some comment"
```
3. Create a secret that you'll deploy to your Rancher host. We are assuming you are using the defaults and be sure to pay attention to the indentation.
```yaml
apiVersion: v1
data:
  ssh-privatekey: 
    <base64 encoded private_key here>
  ssh-publickey: 
    <base64 encoded pub_key here>
kind: Secret
metadata:
  name: alegradi-github-ssh-key
  namespace: fleet-default
type: kubernetes.io/ssh-auth
```

Then create the secret with:
```bash
k create -f filename.yaml
```

4. Create a repo in Rancher and specify your freshly created secret for auth method
5. Add the ssh public key to your git(hub) account so that Rancher can pull the code
6. Watch the magic happen as you update this repo and your changes are mirrored in your Kubernetes cluster