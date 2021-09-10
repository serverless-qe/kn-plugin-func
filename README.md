# OpenShift Serverless Functions

This repository holds the OpenShift Serverless fork of
`knative-sandbox/kn-plugin-func` with modifications needed only for OpenShift.

The upstream CLI uses built in "language packs" that use paketo builders and
buildpacks. This midstream repository replaces the `manifest.yaml` files in
each of these language pack templates with one containing the Red Hat builder
images. These can be found in `./templates`.

## Mirroring upstream

The upstream repo, `knative-sandbox/kn-plugin-func` is mirrored on the 
`release-next` and `release-next-ci` branches, as well as all of the existing
release branches.

In order for mirroring to work correctly, you'll need to have two git remotes
for this repository.

- `upstream` pointing to `knative-sandbox/kn-plugin-func`
- `openshift` pointing to `openshift-knative/kn-plugin-func`

When we are preparing to release a new version of OpenShift Serverless functions
we need to mirror the upstream repository and apply the template modifications.
This is done using the `openshift/release/update-to-head.sh` script. When it runs,
the following steps are taken.

- The upstream is fetched and checked out as the `release-next` branch
- The `openshift` remote `main` branch is pulled and openshift specific files from that branch are applied to the `release-next` branch
- The `release-next` branch is force pushed to the `openshift` remote
- The `release-next` branch is duplicated to `release-next-ci`
- A timestamp file is added to `release-next-ci` branch
- The `release-next-ci` branch is force pushed to the `openshift` remote
- A pull request is created (if it does not already exist) for this change, in order to trigger a CI run
