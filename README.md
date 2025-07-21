# imagevolume
ImageVolume setup using minikube (including custom ImageVolume generation). Currently only supported using container runtime crio>=1.33 on kubernetes>=1.33. Data is loaded in /volume directory on pod.

## minikube setup

    ### 1. Setup minikube. Use kubernetes > v1.33.3 and crio runtime. Enable ImageVolume feature gate
    minikube start --container-runtime=cri-o --feature-gates=ImageVolume=true --kubernetes-version=v1.33.3

    ### 2. Install latest crio runtime manually (required, as ImageVolume feature is still in beta)
    minikube ssh

    # sudo su
    # apt-get update
    # apt-get install -y software-properties-common curl
    # export KUBERNETES_VERSION=v1.33
    # export CRIO_VERSION=v1.33
    # curl -fsSL https://download.opensuse.org/repositories/isv:/cri-o:/stable:/$CRIO_VERSION/deb/Release.key |
    gpg --dearmor -o /etc/apt/keyrings/cri-o-apt-keyring.gpg
    # echo "deb [signed-by=/etc/apt/keyrings/cri-o-apt-keyring.gpg] https://download.opensuse.org/repositories/isv:/cri-o:/stable:/$CRIO_VERSION/deb/ /" |
    tee /etc/apt/sources.list.d/cri-o.list
    # apt-get update
    # apt-get remove -y conmon cri-o cri-o-runc
    # apt-get install -y cri-o cri-tools

    minikube stop
    minikube start

    ### 3. Create imagevolume-pod using template from kubernetes.io (source: https://kubernetes.io/docs/tasks/configure-pod-container/image-volumes/)
    kubectl apply -f imagevolume-default.yaml 

    ### n. Delete minikube
    minikube delete

## build custom imagevolume

    From customimagevolume dir, execute:

    1. Build custom image
    $ docker build -t customimagevolume:latest .
    
    2. Transfer custom image to minikube 
    $ minikube image load customimagevolume:latest

    3. Launch customimagevolume pod
    $ kubectl apply -f imagevolume-custom.yaml
