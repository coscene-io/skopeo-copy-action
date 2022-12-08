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
