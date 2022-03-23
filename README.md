# pipeline-for-deploy-in-eks
Demo of CI/CD pipeline for deploy in EKS

# Part 1: EKS cluster running
- Example: ```eksctl create cluster --name demoeks --version 1.21 --nodegroup-name demoeks-nodes --node-type t3.micro --nodes 4 --managed --profile administrator```

# Part 2: IAM setup
- Create IAM roles for AWS CodePipeline, AWS CodeBuild 
- Create Kubectl Role
    - Create a KubectlRoleForEKS with Policy kubectl_role_policy.json in IAM Console
    - Create Roles for CodeCommit and CodePipeline with CloudFormation Console (Use IAM-roles-cloudformation.yaml and create an EKS-CI-CD-IAM-Stack)
- Edit configmap to give access to KubectlRoleForEKS
    - ```kubectl edit -n kube-system configmap/aws-auth```
    - Add lines from aws-auth-lines-to-add.yaml

# Part 3: Create the Pipeline
- Create a CodeCommit repository in console:
    - Create a repository in AWS CodeCommit ```ekscicdtest```
    - Load main.go, Dockerfile, buildspec.yml and hello-k8s.yml
- Create a pipeline with CodePipeline console
    - name: ```udemyekspipeline```
    - create a project ```udemyeksproject```
    - create an ECR repo ```udemyecrrepo```
- Run the pipeline
    - check ```kubectl get all```
- Delete resources
    - ```eksctl delete cluster --name=demoeks --region=us-east-2 --profile administrator```

# Pipeline Architecture
![Pipeline demo](/architecture/pipeline-architecture-demo.png)