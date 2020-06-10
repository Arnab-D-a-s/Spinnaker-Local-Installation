# Spinnaker-Local-Installation

Install kubectl.
Install helm but pay attention  that you dont need to apply init command in Helm 3. 

curl -O https://raw.githubusercontent.com/spinnaker/halyard/master/install/macos/InstallHalyard.sh
sudo bash ./InstallHalyard.sh -y --user $USER
Check The Halyard version. 


Enable Halyard for Kubernetes Provider
check Halyard Configs 

providers:
kubernetes:
      enabled: true
      accounts:
      - name: my-k8s-v2-account
        requiredGroupMembership: []
        providerVersion: V2
        permissions: {}
        dockerRegistries: []
        context: docker-for-desktop


Spinnaker is deployed to the local Kubernetes cluster, with each microservice deployed independently.

 - hal config deploy edit --type distributed --account-name $ACCOUNT


Get the storage to store the metadata to be used by Spinnaker

helm install my-s3-storage --set persistence.size=8Gi --set accessKey=$MINIO_ACCESS_KEY,secretKey=$MINIO_SECRET_KEY --namespace storage stable/minio

echo $MINIO_SECRET_KEY | hal config storage s3 edit --endpoint $ENDPOINT --access-key-id $MINIO_ACCESS_KEY --secret-access-key


Set spinnaker.s3.versioning: false in the deployment profiles for the front50-local application. As Minio would not support Versioning 



Set up spinnaker version and hal deploy apply to install that 

hal deploy connect to port forward
