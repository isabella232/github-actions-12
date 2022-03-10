# github-actions
Repository for shared GitHub Actions

This repository contains reusable github action jobs that can be used in the GitHub action workflows in other repositories.

## How to use
Reference required steps in your workflow `yml` files.

Check the latest version in the [releases](https://github.com/e-conomic/github-actions/releases) and suffix the actions with it.

The current documentatino assumes that the latest version is `v4`

## Actions
* docker-build-push
* polaris-sast
* upload-docs

The following examples assume that you have included a github-actions repo checkout in the previous step.

### docker-build-push
Build docker image and publish

```yaml
- name: Docker build and push
  uses: e-conomic/github-actions/docker-build-push@v4
  with:
    password: ${{ secrets.DEVECONOCM_GCR_RW }}
    tags: eu.gcr.io/dev-econo-cm/<project-name-and-version>
```
#### User customizable options
`tags` - replace **<project-name-and-version>** with you own value.
ex.: `companies:${{ github.ref_name }}-${{ env.SHA7 }}`
  
  By default github context does not provide short SHA of the commit. There are multiple ways to calculate short SHA and this is just one example. You can add an additional step before calling `docker-build-push` to generate a string formatted SHA containing 7 characters:
```yaml
- name: Get commit short SHA
  run: echo SHA7=${GITHUB_SHA::7} >> $GITHUB_ENV
```

### polaris-sast
Run polaris static application security testing. The action runs on the root folder of the cloned application repository.
```yaml
- name: Static application security testing
  uses: e-conomic/github-actions/polaris-sast@v4
  with:
    api_url: ${{ secrets.POLARIS_API_URL }}
    access_token: ${{ secrets.POLARIS_ACCESS_TOKEN }}
```
#### User customizable options
None
 
### upload-docs
Upload documentation to docs.e-conomic.ws
```yaml
- name: Upload docs
  uses: e-conomic/github-actions/upload-docs@v4
  with:
    docs_bucket_sa: ${{ secrets.DOCS_BUCKET_SA }}
```
#### User customizable options
None
  
## How to contribute
If you want to add a new shareable action, please create a pull request containing:
* Code updates (If you are creating a new action, place it in a new directory)
* Update of the readme.md reflecting your changes if necessary
  
Once the  changes are merged in to the `master` branch, create a release with the new versio number.
