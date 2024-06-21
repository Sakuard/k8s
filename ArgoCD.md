> Why ArgoCD ?

#### When use Gitlab-Agent to handle CD's job.
1. config setup in `.gitlab-ci.yml`
2. deployment status check


```yaml=
# config setup in .gitlab-ci.yml
deploy:
  image:
    name: bitnami/kubectl:latest
    entrypoint: ['']
  script:
    - kubectl config get-contexts
    - kubectl config use-context path/to/agent/project:agent-name
    - kubectl get pods
```
```bash=
# deployment status check
kubectl get all
```
#### ArgoCD handle these with a UI friendly interface
![Argo-UI](https://hackmd.io/_uploads/BJu1swG8R.png)

### GitLab-Agent vs ArgoCD
![Agenv-Argo](https://hackmd.io/_uploads/SJG3GdML0.png)
### Deploy with ArgoCD
1. install ArgoCD in kubernetes cluster
2. Login ArgoCD
3. Deploy setup

### Install
```bash=
# install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# edit ArgoCD-svc
kubectl edit svc argocd-server -n argocd
```
### Login ArgoCD
1. Set ArgoCD-Server into LoadBalancer
2. Get init admin password
```yaml=
# update `sepc.type` to LoadBalancer
spec:
  type: LoadBalancer
```
```bash=
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode; echo
```
Login with admin
![Argo-Login](https://hackmd.io/_uploads/SyQ1a_MIA.png)

### Deploy setup
[argo-demo](./src/argo/argo-demo.gif)


![image](https://hackmd.io/_uploads/SJVSZIG8C.png)
