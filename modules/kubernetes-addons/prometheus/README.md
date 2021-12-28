# Prometheus Helm Chart

###### Instructions to upload Prometheus Docker image to AWS ECR

Step1: Get the latest docker image from this link

        https://github.com/prometheus-community/helm-charts/blob/main/charts/prometheus/values.yaml

Step2: Download the docker image to your local Mac/Laptop

        $ docker pull quay.io/prometheus/prometheus:v2.26.0
        $ docker pull quay.io/prometheus/alertmanager:v0.21.0
        $ docker pull jimmidyson/configmap-reload:v0.5.0
        $ docker pull quay.io/prometheus/node-exporter:v1.1.2
        $ docker pull prom/pushgateway:v1.3.1


Step3: Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:

        $ aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin <account id>.dkr.ecr.eu-west-1.amazonaws.com

Step4: Create an ECR repo for each image mentioned in Step2 with the same in ECR. See example for

        $ aws ecr create-repository \
              --repository-name quay.io/prometheus/prometheus \
              --image-scanning-configuration scanOnPush=true

Repeat the above steps for other 4 images

Step5: After the build completes, tag your image so, you can push the image to this repository:

        $ docker tag quay.io/prometheus/prometheus:v2.26.0 <accountid>.dkr.ecr.eu-west-1.amazonaws.com/quay.io/prometheus/prometheus:v2.26.0

Repeat the above steps for other 4 images

Step6: Run the following command to push this image to your newly created AWS repository:

        $ docker push <accountid>.dkr.ecr.eu-west-1.amazonaws.com/quay.io/prometheus/prometheus:v2.26.0

Repeat the above steps for other 4 images

### Instructions to download Helm Charts

Helm Chart

    https://github.com/prometheus-community/helm-charts/blob/main/charts/prometheus/values.yaml


<!--- BEGIN_TF_DOCS --->
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0

Permission is hereby granted, free of charge, to any person obtaining a copy of this
software and associated documentation files (the "Software"), to deal in the Software
without restriction, including without limitation the rights to use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Requirements

No requirements.

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | n/a |
| <a name="provider_helm"></a> [helm](#provider\_helm) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [helm_release.prometheus](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_amazon_prometheus_ingest_iam_role_arn"></a> [amazon\_prometheus\_ingest\_iam\_role\_arn](#input\_amazon\_prometheus\_ingest\_iam\_role\_arn) | Amazon Managed Prometheus Ingest IAM Role ARN | `string` | `null` | no |
| <a name="input_amazon_prometheus_ingest_service_account"></a> [amazon\_prometheus\_ingest\_service\_account](#input\_amazon\_prometheus\_ingest\_service\_account) | Amazon Managed Prometheus Ingest Service Account | `string` | `null` | no |
| <a name="input_amazon_prometheus_workspace_id"></a> [amazon\_prometheus\_workspace\_id](#input\_amazon\_prometheus\_workspace\_id) | Amazon Managed Prometheus Workspace ID | `string` | `null` | no |
| <a name="input_helm_config"></a> [helm\_config](#input\_helm\_config) | n/a | `any` | `{}` | no |
| <a name="input_manage_via_gitops"></a> [manage\_via\_gitops](#input\_manage\_via\_gitops) | Determines if the add-on should be managed via GitOps. | `bool` | `false` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_argocd_gitops_config"></a> [argocd\_gitops\_config](#output\_argocd\_gitops\_config) | Configuration used for managing the add-on with ArgoCD |

<!--- END_TF_DOCS --->