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
