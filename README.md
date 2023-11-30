# Craete a instance for Jenkins
# Installing jenkins Ubuntu
**https://www.jenkins.io/doc/book/installing/linux/**

**Installing docker on jenkins**
```
apt update -y
apt update install docker.io -y
usermod -aG docker jenkins
````


# Installing Argocd on k8's
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get svc -n argocd
kubectl edit svc argocd-server -n argocd
// Replace ClusterIP to LoadBalancer or NodePort
kubectl get svc -n argocd  //(Copy the LoadBalancerIP:80 and paste on browser /or/ NodeportIP:Nodeport paste on browser Nodeport look like 30000)

//Argocd username admin for password run below code
kubectl get secrets -n argocd
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode  //copy and paste the password on argocd gui
//change the password after login to argocd   

//Install ArgoCD CLI in Cloud Shell (GCP)


wget https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64 -O ~/argocd
chmod +x ~/argocd
mkdir -p ~/bin
mv ~/argocd ~/bin/
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

//login to argocd to add k8s to argocd //ip of argocd username and password
argocd login 34.171.45.65:80 --username admin

//enter 'Y' then enter your argocd updated password


//Add k8s to Argocd

kubectl config get-contexts



argocd cluster add gke_kubernetes-405017_us-central1-a_my-first-cluster-1 --name mycluster
// replace mycluster and gke_kubernetes-405017_us-central1-a_my-first-cluster-1 replace this ur cluster name (u will get by running 'kubectl config get-contexts')

argocd cluster list
```

