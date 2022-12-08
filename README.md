# Skopeo Copy Action

Reference: [Copying images](https://github.com/containers/skopeo#copying-images)

Copy image from src to dst.

## Inputs

| Input parameter name | Description                                                              | Example                                                   |
|----------------------|--------------------------------------------------------------------------|-----------------------------------------------------------|
| src-image            | source image with tag to copy from                                       | coseus.azurecr.io/platform:latest                         |
| src-creds            | credentials with format: [USERNAME]:[PASSWORD] to pull source image      | username:password                                         |
| dst-image            | destination image with tag to copy to                                    | registry.cn-hangzhou.aliyuncs.com/coscene/platform:latest |
| dst-creds            | credentials with format: [USERNAME]:[PASSWORD] to push destination image | username:password                                         |

## Troubleshooting

1. You MUST ensure that the content of the credentials `[USERNAME]:[PASSWORD]` DOES NOT contain any colons(`:`) other
   than the separator one. Otherwise, it is likely that the registry will not be accessed correctly. Of course, we can
   extend the `cred` parameters to `registry`, `username` and `password` to avoid this problem. But this increases the
   complexity of this action.
2. We tend to use Skopeo inside GFW to sync the outside image into the registry inside GFW. When you run this action on
   self-hosted runner, you MUST ensure that Skopeo installed. Here is an example to use an `action-runner-dind` with
   Skopeo.

```dockerfile
# You can mirror actions-runner-dind:ubuntu-22.04 first by the following cmd:
# skopeo copy --dest-creds <creds> \ 
# docker://ghcr.io/actions-runner-controller/actions-runner-controller/actions-runner-dind@sha256:cdbd5684259efe90b0d88e656ef2a154f34ce7b1104d892ca6061d96055bc94b \
# docker://registry.cn-hangzhou.aliyuncs.com/coscene/actions-runner-dind:ubuntu-22.04 
FROM registry.cn-hangzhou.aliyuncs.com/coscene/actions-runner-dind:ubuntu-22.04

RUN sudo apt-get update -y \
    && sudo apt-get install -y skopeo
```

## A little best practice

Here is a typical example about maintaining images between the registry outside GFW and the one inside GFW.

We build docker image on GitHub-hosted runner and sync image from the registry outside the GFW to the registry inside
GFW on self-hosted runner. If the synchronization fails on self-hosted runner, the job will be done on GitHub-hosted
runner as a fallback. This has proven to be the most economically efficient (cost of self-built containers) and
time-consuming overall. Practice has shown that it is more difficult and insurmountable to push and pull data from outside the GFW to inside the GFW than to push and pull data from inside the GFW through a reliable network proxy.
