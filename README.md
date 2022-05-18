# finderly/actions

Reusable GitHub Action workflows

## [image-build.yml](.github/workflows/image-build.yml)

Builds container images with docker buildx, pushes to ECR. Runs on `self-hosted`,
`physical` tagged runners.

### inputs

| Name           | Type   | Req? | Default | Description                                                                |
|:---------------|:-------|:-----|:--------|:---------------------------------------------------------------------------|
| `aws-region`   | string | yes  |         | AWS region for hosting container images                                    |
| `aws-username` | string | no   | `AWS`   | Username for AWS Elastic Container Registry (ECR) login                    |
| `registry`     | string | yes  |         | AWS ECR address                                                            |
| `image`        | string | yes  |         | Container image name to build, usually based on repo                       |
| `tag`          | string | yes  |         | Container image tag, usually a commit sha                                  |
| `build-target` | string | yes  |         | Container image build target. Dev image automatically appended with `-dev` |


### secrets

| Name                    | Type   | Required? | Description                                     |
|:------------------------|:-------|:----------|:------------------------------------------------|
| `aws-access-key-id`     | string | yes       | AWS access key id for configuring `aws` cli     |
| `aws-secret-access-key` | string | yea       | AWS secret access key for configuring `aws` cli |
| `ssh-private-key`       | string | yes       | SSH private key exposed to the container build  |

### outputs
 none

### Usage

In your workflow reference this reusable workflow like this:
```yaml
jobs:
  build-and-push-my-image:
    name: My-App
    uses: finderly/actions/.github/workflows/image-build.yml@main
    with:
      aws-region: "region-1"
      registry: "123456789012.dkr.ecr.region-1.amazonaws.com"
      image: "my-app"
      tag: "${{ github.sha }}"
    secrets:
      aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
      aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      ssh-private-key: ${{ secrets.CI_USER_SSH_PRIVATE_KEY }}
```
