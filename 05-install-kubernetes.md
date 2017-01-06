Steps to install a minimal Kubernetes setup on bare metal

http://kubernetes.io/docs/getting-started-guides/kubeadm/

To allow the single node to accept PODs:

`kubectl taint nodes --all dedicated-`
