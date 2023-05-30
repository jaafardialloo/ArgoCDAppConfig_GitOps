# ArgoCDAppConfig_GitOps
This repository is for me to get may hands wet  with gitOps and ArgoCD .

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. It is designed to automate the deployment and management of applications in a Kubernetes cluster, ensuring that the desired state of the applications matches the actual state.

# Steps for configuration

In order to experiment with argoCD using this repository, you can follow the below steps : 

- Configuring a K8s cluster. For this you may use Kind (k8s in docker) in case you want to go faster or you can't get a phisical cluster for more info you can visit : https://kind.sigs.k8s.io/docs/user/configuration/.
- Configure kubectl : Kubernetes command-line tool, https://kubernetes.io/docs/tasks/tools/ .
- Installing ArgoCD : for this you have to execute the next two commandes within the already prepared cluster :
            - kubectl create namespace argocd
            - kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml

for more details refer to the official documentation  : https://argo-cd.readthedocs.io/en/stable/getting_started/

- Once above steps done, your next task will be to initiate a github repository that contains the desired state of your deployment. This repository is the one and only trusted source.
- Then you will need to create an application in argoCD and link it to git repository, for this argoCD provides two options : 
                         - Web UI and CLI: Argo CD provides a web-based user interface (UI) and a command-line interface (CLI) for managing applications, monitoring deployments, 
                                           and interacting with the cluster.
 
During my practical journey with argoCD, i encountred some erros that i think deserve to be mentioned in case you encounter the same : 

   - ComparisonError: error synchronizing cache state (failed to sync cluster ) : This is due to the lack of authoritieson, for the Argo CD service account argocd-application-controller to apply some actions on some k8s resources such as priorityclasses, persistentvolumes ...etc
    for this issue you have to create two k8s objects : 
            - ClusterRole : for defining a role .
            - ClusterRoleBinding : binding the created role to a service account.
            
With everything done you will get argoCD applications for each of your deployments displayed all on argoCD UI hence facilitating monitoring, saves time and enhance your productivity. Also argoCD will make sure that deployed state is always synchronized with the desired state which is hosted on your git repository.
