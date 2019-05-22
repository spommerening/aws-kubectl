The Docker Image contains the aws-cli and kubectl.
It is used to update the AWS ECR credentials periodically in a Kubernetes cluster.

# Setup

You need to set your credentials in the aws-secrets.yml.
Also you need to set your AWS_ACCOUNT, AWS_REGION and NAMESPACES in ecr-cron.yml.
Afterwords run:

	aws.sh

Afterwords you should be able to see the cron job with:

	kubectl get cronjobs -n infrastructure

## IAM integration (optional)

If you are running on AWS but require a secret type dockerconfigjson generated
regardless of IAM roles and such just remove the access key id and secret access key
from `aws_secrets.yml`.

Now add the following configuration to `ecr_cron.yml` (for use with `kube2iam`):

````
      [...]
      template:
        metadata:
          annotations:
            iam.amazonaws.com/role: arn:aws:iam::447494836616:role/ECR-ReadOnly
            prometheus.io.scrape: "true"
        spec:
          serviceAccountName: ecr-update-service-account
      [...]
````

# Thanks

This is based on the work of @xynova and Mike Petersen <mike@odania-it.de>.

# References

* https://medium.com/@xynova/keeping-aws-registry-pull-credentials-fresh-in-kubernetes-2d123f581ca6
* https://github.com/Odania-IT/aws-kubectl
