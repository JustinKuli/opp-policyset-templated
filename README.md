# opp-policyset-templated

1. Install the policy generator as a Kustomize plugin on your machine:
https://github.com/stolostron/policy-generator-plugin/blob/main/README.md#install-from-the-github-release

2. Log in to your ACM Hub

3. Create the PolicySet (as configured in this repo, the policies will only be applied to 
**managed** clusters): `cd policyset && kustomize build --enable-alpha-plugins | oc apply -f -`

4. Create the `opp-policy-config` configmap on the managed cluster, and the policies should start
progressing towards compliance (before this, the templates that require that configmap will
effectively pause most of those policies on that cluster).
