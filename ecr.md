Hello everyone! In this guide, I will try to walk you through working with ECR while we are on our project.
Let's start with pushing image to ECR. All of the images will be hosted in ```
http://074368059797.dkr.ecr.eu-central-1.amazonaws.com/nikitasadok```
Since we cannot use roles for pushing because of GitHub Action limitations, every member of the team will have personal IAM user
which can perform only pushes to the repo. If you want your creds just text me :). 
Pulling. It gets a little more tricky with that. First, you need to have a user who can, at least, read from the ECR repo. 
After that, you need to give me the username of that user as well as the AWS account ID. I will share the IAM role with you. 
Then, you need to go to IAM -> Users -> your_pulling_user -> Policies -> add inline policy. Then, you need to choose JSON and 
paste the following:
```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "sts:AssumeRole",
        "Resource": "arn:aws:iam::074368059797:role/ecr-access"
    }
}
```

This basically allows your user to switch to the role and to do something with the privileges provided by another account.
Now, you should be able to pull from the repo. Now it is time to set up registry-creds chart which will help us with pulling
images in our cluster. 
First, you need to add the repository with the registry-creds chart to helm:
```
helm repo add kir4h https://kir4h.github.io/charts/
```
After that, you need to install the helm chart in your cluster. You can do that with the following command:
```
helm install registry-creds --set ecr.enabled=true --set-string ecr.awsAccount="074368059797" 
--set-string ecr.awsRegion="eu-central-1" --set-string ecr.awsAccessKeyId="access-key-of-your-pulling-user" 
--set-string ecr.awsSecretAccessKey="secret-access-key-of-your-pulling-user"
--set-string ecr.awsAssumeRole="arn:aws:iam::074368059797:role/ecr-access" kir4h/registry-creds
```

Now you are all set up! However, you still need to add imagePullSecrets to your deployment.
Let's do this:
```
imagePullSecrets:
- name: awsecr-cred
```

That's it!
