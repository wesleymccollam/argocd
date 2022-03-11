# Pre-reqs #
1) create ns for housing Argo CD K8 resources
2) ``` kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml ```
3) ```brew install argocd ``` to install Argo CD CLI utility
4) ```kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}' ```
5) ``` kubectl port-forward svc/argocd-server -n argocd 8080:443 ```
6) ``` kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo ```

# Steps to run Ping Helm charts to manage K8 deployments #
1) New App
2) Before assigning to a project, make sure you decide whether or not just to use default
3) Decide on Sync Policy preferences
4) Source Repository URL, select Helm
- 4a) Enter in https://helm.pingidentity.com
- 4b) Chart, Select ping-devops
- 4c) If new, pick latest version of chart at very top of version list
5) Select Cluster URL and namespace where deployment is desired

# Run this #
1) ```pingctl k8s generate devops-secret | kubectl apply -f - -n <namespace from step 5 of previous section>```
2) Click on application just created, select the Application box to the far left
3) Go to PARAMETERS tab, EDIT
- 3a) Under VALUES FILES, click the empty value, select values.yaml
- 3b) Add this content into VALUES box
```
  global:
    envs:
      PING_IDENTITY_ACCEPT_EULA: "YES"
```
4) Save
5) Edit any parameters for desired values to apply from the PARAMETERS list.
6) Exit menu

# Apply Changes #
1) Select REFRESH
2) SYNC
3) If removing values, you will need to check the box "PRUNE" when syncing changes

# Notes #
- Be sure to change the password later in https://replace_this_hostname:8080/user-info
