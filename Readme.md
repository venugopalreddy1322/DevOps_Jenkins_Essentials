Jenkins Notes
---------------------
Plugins installed
1. Docker
2. Jenkins --> and set path
    Manage Jenkins --> Tools --> Terraform --> (uncheck Install automatically)--> Install directory:  give the directory('$which terraform' from terminal) /usr/bin/terraform

Credentials added
Global:
1. GitHub:
    Username: Your GitHub username (venugopalreddy1322)
    password: Settings--Developer-settings-personaltocken
2. DockerHub: same credentials as DockerHub
    username:
    Password:
½
Connect vagrant machine to AWS
----------------------------------------------------
1. $ aws configure on Vagrant machine
2. jenkins user already created while installing jenkins
3. add jenkins user to sudo group --> $ sudo usermod -aG sudo jenkins
4.  Here are the steps to create the directory and copy the credentials file:

Create the Directory:

Create the .aws directory in the Jenkins home directory:

sh
sudo mkdir -p /var/lib/jenkins/.aws
Copy the Credentials File:

Copy the credentials file to the newly created directory:

sh
sudo cp /home/vagrant/.aws/credentials /var/lib/jenkins/.aws/credentials
Set Permissions:

Set the ownership and permissions for the copied credentials file to ensure the Jenkins user has access:

sh
sudo chown -R jenkins:jenkins /var/lib/jenkins/.aws
sudo chmod 600 /var/lib/jenkins/.aws/credentials
These steps should ensure that the Jenkins user can access the AWS credentials file.

NOTE:
In terraform sh 'cd myfolder' will not continue to the next line it will come back to the root directory. so:
1. sh """
        cd myfolder
        terraform init
        terraform plan
        terraform apply --auto-approve
    """
    sh 'pwd'---> here it will be back to the root directory again
    (OR)
    sh 'cd myfolder; terraform init; terraform plan; terraform apply --auto-approve'

2. use: dir()
    steps {
        dir('myfolder') {
            -----
            -----
        }
    }


## To change Jenkin's default port 8080 to custom port for ex:8081
### Step1:
1. Edit the Jenkins configuration file:

sh
sudo nano /etc/default/jenkins

2. Find the line that defines the port:

JENKINS_PORT=8080

3. Change it to:

JENKINS_PORT=8081

4. Save and exit Nano (Ctrl + X, then Y, then Enter).

5. Restart Jenkins to apply the changes:

sh
sudo systemctl restart jenkins

### Step2:
1. Edit the Jenkins systemd service file:

sh
sudo nano /usr/lib/systemd/system/jenkins.service

2. Find the line that defines the environment variable:

Environment="JENKINS_PORT=8080"
3. Change it to:

Environment="JENKINS_PORT=8081"

4. Save and exit Nano (Ctrl + X, then Y, then Enter).

5. Reload systemd daemon:

sh
sudo systemctl daemon-reload

6. Restart Jenkins:

sh
sudo systemctl restart jenkins

Now, Jenkins should be running on port 8081. If it still doesn’t work, check if another process is using port 8081 with:

sh
sudo netstat -tulnp | grep 8081