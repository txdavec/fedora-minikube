# fedora-minikube
setup minikube in fedora with kvm2

```
## cat /etc/fedora-release
## Fedora release 36 (Thirty Six)

sudo dnf -y update
sudo dnf -y install vim-enhanced

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm

sudo dnf -y install minikube-latest.x86_64.rpm

sudo dnf -y install @virtualization

sudo systemctl start libvirtd
sudo systemctl enable libvirtd

sudo usermod --append --groups libvirt $(whoami)

minikube config set driver kvm2
minikube delete
minikube start

mkdir ~/.bashrc.d
vi ~/.bashrc.d/k8s
# add
alias kubectl="minikube kubectl --"

kubectl get nodes
minikube dashboard

# running a test pod
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: testpod
  name: testpod
spec:
  containers:
  - args:
    - sh
    - -c
    - echo "Hello Tom!"
    image: busybox
    name: testpod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
EOF

kubectl logs testpod

kubectl delete pod testpod

minikube status
minikube stop

# to clean up everything, VMs, settings
minikube delete --all --purge

```
