another tenant onboarding option: 
- https://docs.vmware.com/en/Application-Single-Sign-On-for-VMware-Tanzu/1.0/appsso/GUID-appendix-tap-dev-ns-setup-example.html

```
export WORKLOADS_NAMESPACE="workloads"
export GIT_REPOSITORY_HOSTNAME="github.com"
export GIT_REPOSITORY_SSH_PRIVATE_KEY="foo"
export GIT_REPOSITORY_SSH_PUBLIC_KEY="bar"
export GIT_REPOSITORY_KNOWN_HOSTS="$(ssh-keyscan github.com)"
```

# Deploy kapp as per namespace spec yaml
```
ytt \
   --data-value-file=git_repository_hostname=<(echo "${GIT_REPOSITORY_HOSTNAME}") \
   --data-value-file=ssh_private_key=<(echo "${GIT_REPOSITORY_SSH_PRIVATE_KEY}") \
   --data-value-file=ssh_public_key=<(echo "${GIT_REPOSITORY_SSH_PUBLIC_KEY}") \
   --data-value-file=known_hosts=<(echo "${GIT_REPOSITORY_KNOWN_HOSTS}") \
   --data-value=namespace="${WORKLOADS_NAMESPACE}" \
   --file tenant-template.yml
```

The intent is to just use ytt to resolve yaml placeholders, source control to a subfolder (named by namespace name) in a gitops managed repo and let teams PR if they want to change any of the onboarding resource initially stood up.

