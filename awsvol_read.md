Kubernetes/kops: error attaching EBS volume to instance. You are not authorized to perform this operation. Error 403

In kops 1.8.0-beta.1, master node requires you to tag the AWS volume with:

KubernetesCluster: <clustername-here>

If you have created the k8s cluster using kops like so:

kops create cluster --name=k8s.yourdomain.com [other-args-here]

your tag on the EBS volume needs to be

KubernetesCluster: k8s.yourdomain.com

And the policy on master would contain a block which would contain:

{
  "Sid": "kopsK8sEC2MasterPermsTaggedResources",
  "Effect": "Allow",
  "Action": [
    "ec2:AttachVolume",
    "ec2:AuthorizeSecurityGroupIngress",
    "ec2:DeleteRoute",
    "ec2:DeleteSecurityGroup",
    "ec2:DeleteVolume",
    "ec2:DetachVolume",
    "ec2:RevokeSecurityGroupIngress"
  ],
  "Resource": [
    "*"
  ],
  "Condition": {
    "StringEquals": {
      "ec2:ResourceTag/KubernetesCluster": "k8s.yourdomain.com"
    }
  }
}
The condition indicates that master-policy has privilege to only attach volumes which contain the right tag.
