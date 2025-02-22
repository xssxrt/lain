# Notes
## Sources
You can just leverage the default git source if you want to vendor some charts.


## Commands
### kubeadm
```bash
sudo kubeadm init \
--skip-phases=addon/kube-proxy \
--pod-network-cidr=10.42.0.0/16
```
### cilium
cilium install \
--set ipam.operator.clusterPoolIPv4PodCIDRList="10.42.0.0/16" \
--set ipv4NativeRoutingCIDR=10.42.0.0/16 \
--set ingressController.enabled=true \
--set ingressController.loadbalancerMode=dedicated

### Bootstrap command
```bash
flux bootstrap github \
--token-auth \
--owner=xssxrt \
--branch=main \
--components source-controller,kustomize-controller,helm-controller,notification-controller \
--components-extra image-reflector-controller,image-automation-controller \
--private=false \
--personal=true \
--repository=lain \
--path=clusters/lab \
--toleration-keys "node-role.kubernetes.io/control-plane"
```
### Create flux resource 
```bash
flux create hr cloudnative-pg \
--source=GitRepository/flux-system \
--interval=10m \
--chart=charts/vendor/cloudnative-pg/ \
--target-namespace=cloudnative-pg \
--create-target-namespace=true \
--export > release.yaml
```

### SOPS - show diffs in cleartext w/ git
```bash
git config diff.sopsdiffer.textconv "sops decrypt"
```
NB: `.gitattributes` needs to be updated if a different serialzation langauge also needs to have secrets.

### Helm
#### Pull vendor charts to access to them locally
```bash
cd ~/code/lain/charts/vendor/
# cloudnative-pg as an example
helm pull cnpg/cloudnative-pg --untar
# creates a dir cloudnative-pg with the helm chart's contents
```

