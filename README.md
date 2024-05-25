## Devops-fully-automated-infracture-deployment-with-Jenkins
Fully automated and secured Terraform infra pipeline

Testing the webhook.....

## CICD Infra setup
1) ###### GitHub setup
    Fork GitHub Repository by using the existing repo "effulgencetech-devops-fully-automated-infra" (https://github.com/Michaelgwei86/jenkins_with_terraform_deployment.git)     
    - Go to GitHub (github.com)
    - Login to your GitHub Account
    - **Fork repository "effulgencetech-devops-fully-automated-infra" (https://github.com/Michaelgwei86/jenkins_with_terraform_deployment.git) & name it "effulgencetech-devops-fully-automated-infra.git"**
    - Clone your newly created repo to your local

2) ###### Jenkins
    - Create an **Amazon Linux 2 VM** instance and call it "Jenkins"
    - Instance type: t2.large
    - Security Group (Open): 8080, 9100 and 22 to 0.0.0.0/0
    - Key pair: Select or create a new keypair
    - **Attach Jenkins server with IAM role having "AdministratorAccess"**
    - Launch Instance
    - After launching this Jenkins server, attach a tag as **Key=Application, value=jenkins**
    - SSH into the instance and Run the following commands in the **jenkins-install.sh** file

3) ###### Slack 
    - **Join the slack channel https://devopswithmike.slack.com/archives/C0590M8QZ97**
    - **Join into the channel "#developers"**

### Jenkins setup
1) #### Access Jenkins
    Copy your Jenkins Public IP Address and paste on the browser = **ExternalIP:8080**
    - Login to your Jenkins instance using your Shell (GitBash or your Mac Terminal)
    - Copy the Path from the Jenkins UI to get the Administrator Password
        - Run: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
        - Copy the password and login to Jenkins
    - Plugins: Choose Install Suggested Plugings 
    - Provide 
        - Username: **admin**
        - Password: **admin**
        - Name and Email can also be admin. You can use `admin` all, as its a poc.
    - Continue and Start using Jenkins

2)  #### Pipeline creation
    - Click on **New Item**
    - Enter an item name: **effulgencetech-app-infra-pipeline** & select the category as **Pipeline**
    - Now scroll-down and in the Pipeline section --> Definition --> Select Pipeline script from SCM
    - SCM: **Git**
    - Repositories
        - Repository URL: FILL YOUR OWN REPO URL (that we created by importing in the first step)
        - Branch Specifier (blank for 'any'): */main
        - Script Path: Jenkinsfile
    - Save

3)  #### Plugin installations:
    - Click on "Manage Jenkins"
    - Click on "Plugin Manager"
    - Click "Available"
    - Search and Install the following Plugings "Install Without Restart"        
        - **Slack Notification**

4)  #### Credentials setup(Slack):
    - Click on Manage Jenkins --> Manage Credentials --> Global credentials (unrestricted) --> Add Credentials
        1)  ###### Slack secret token (slack-token)
            - Kind: Secret text            
            - Secret: lXpiMy7yGJLm9V6OsMmdkKVS
            - ID: Slack-token
            - Description: Slack-token
            - Click on Create                

        2)  #### Configure system:
            - Click on Manage Jenkins --> Configure System

            1)  - Go to section Slack
                - Workspace: **devopswithmike** (if not working try with Team-subdomain devopswithmike)
                - Credentials: select the slack-token credentials (created above) from the drop-down    


### Performing continous integration with GitHub webhook

1) #### Add jenkins webhook to github
    - Access your repo **jenkins_with_terraform_deployment** on github
    - Goto Settings --> Webhooks --> Click on Add webhook 
    - Payload URL: **htpp://REPLACE-JENKINS-SERVER-PUBLIC-IP:8080/github-webhook/**    (Note: The IP should be public as GitHub is outside of the AWS VPC where Jenkins server is hosted)
    - Click on Add webhook

2) #### Configure on the Jenkins side to pull based on the event
    - Access your jenkins server, pipeline **effulgencetech-app-infra-pipeline**
    - Once pipeline is accessed --> Click on Configure --> In the General section --> **Select GitHub project checkbox** and fill your repo URL of the project effulgencetech-devops-fully-automated.
    - Scroll down --> In the Build Triggers section -->  **Select GitHub hook trigger for GITScm polling checkbox**

Once both the above steps are done click on Save.


### Codebase setup

1) #### For checking the Gitbut webhook uncomment lines 42-51 in main.tf file
    - Go back to your local, open your "jenkins_with_terraform_deployment" project on VSCODE
    - Open "main.tf file" uncomment lines   
    - Save the changes in both files
    - Finally push changes to repo
        `git add .`
        `git commit -m "relevant commit message"`
        `git push`


## Finally observe the whole flow and understand the integrations
# Happy learning from Hilltop  Consultancy
