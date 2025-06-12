## Destroy a Kubernetes cluster in EKS using eksctl command
Either use cluster.yaml or cluster name in the Jenkinsfile for destruction of EKS cluster.

#### Create a Jenkinsfile for running eksctl command for deployment of EKS cluster:
1. Create a pipeline.
2. Set relevant environment variables.
3. Create various stages within the pipeline:
   - Checkout code from the repository.
   - Install kubectl.
   - Install eksectl.
   - Destroy the EKS cluster using eksctl command.
   - NOTE that the scripts cannot be executed using sudo or root access.
  
#### Setup Jenkins pipeline:
1. Login to the Jenkins server.
2. Install necessary plugins using Manage Jenkins -> Plugins option.
3. Add Github credentials in Jenkins -> Manage Jenkins -> Credentials -> Add Credentials -> Kind (Username and Password).
4. Add DockerHub credentials in Jenkins -> Manage Jenkins -> Credentials -> Add Credentials -> Kind (Username and Password).
5. Add AWS credentials in Jenkins -> Manage Jenkins -> Credentials -> Add Credentials -> Kind (AWS Credentials).
6. Create a Jenkins pipeline using **New Item** option.
7. Set this repository as the SCM source with necessary GIT settings.
8. Set the Script Path as Jenkinsfile using Pipeline -> Pipeline script from SCM section.
9. Save the changes and click **Build Now** to trigger the pipeline.
10. Check the Console Output associated with the lastest job for verification.
11. Also verify the CloudFormation stack progress and/or EKS cluster destruction progress in the AWS management console in the selected region.
12. Additionally, you can setup a Webhook on the repository and enable **GitHub hook trigger for GITScm polling** option in the pipeline for automatic pipeline triggers whenever changes are pushed to the repository.
13. Cluster Name can be added as a Paramter in the pipeline to make the eksctl destoy command independent of cluster.yaml file.
