# Notes
## Commands
### Bootstrap command
```
flux bootstrap github \
--token-auth \
--owner=xssxrt \
--branch=main \
--components source-controller,kustomize-controller,helm-controller,notification-controller \
--components-extra image-reflector-controller,image-automation-controller \
--private=false \
--personal=true \
--repository=lain \
--path=clusters/lab
```

### SOPS - show diffs in cleartext w/ git
```
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

