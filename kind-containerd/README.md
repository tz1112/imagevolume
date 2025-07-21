needs kind>=0.29.0, cause that includes containerd=2.1.1 and kubernetes=1.33.1

see https://sestegra.medium.com/using-oci-image-volume-source-in-kubernetes-pods-684069792b32


1. Deploy kind cluster
kind create cluster --config config.yaml

2. Deploy default imagevolume pod
kubectl apply -f imagevolume-default.yaml

3. Custom image
docker build -t customimagevolume:v1 .
kind load docker-image customimagevolume:v1