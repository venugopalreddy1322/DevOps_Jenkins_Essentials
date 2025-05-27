# Jenkins Notes:


## Jenkins Setup & Configuration
## Install the necessary softwares
- **Jenkins**
  - Go to official Jenkins website to [Install Jenkins](https://www.jenkins.io/doc/book/installing/)

- **Terraform**
  - Go to official Hasicorp website to [Install Terraform](https://developer.hashicorp.com/terraform/install)

- **Docker**
  - To install Docker on Amazon Linux 2 or Amazon Linux 2023 [Install Docker](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-docker.html)  
  - After installation add ec2-user and jenkins to docker group
   ```sh
   sudo usermod -aG docker jenkins ec2-user
   ```
   
- **Terraform Configuration on Jenkins**  
  - Navigate to **Manage Jenkins** → **Tools** → **Terraform**  
  - Uncheck **Install automatically**  
  - Set installation directory to `which terraform` output (e.g., `/usr/bin/terraform`)

## Installed Plugins for the demos
- **Docker**
- **Docker Pipeline Plugin**
 🔹 Enables docker.build(...) and docker.withRegistry(...) functionality.
 
- **Terraform Configuration**  
  - Navigate to **Manage Jenkins** → **Tools** → **Terraform**  
  - Uncheck **Install automatically**  
  - Set installation directory to `which terraform` output (e.g., `/usr/bin/terraform`)

## Global Credentials Added
- **GitHub**  
  - Username: `<your GitHub username>` (e.g., `venugopalreddy1322`)  
  - Personal access token: _Settings → Developer Settings → Personal Token_
- **DockerHub**  
  - Username: _same as DockerHub login_  
  - Password: **DockerHub password**

## NOTE: While running the Jenkins job, if you encounter an error saying permissin denied to jenkins user to execute docker commands:
- There might be several reasons:
   - check wheteher jenkins user added to docker group
      To add ``` sudo chmod -aG docker jenkins ```
      to check ``` groups jenkins ```
   - Change Permissions for Docker Socket
      Try explicitly granting access:
   ```bash
      sudo chmod 666 /var/run/docker.sock
   ```
   This will allows non-root users (like Jenkins) to connect to Docker.
   
## Connecting a Vagrant Machine to AWS
1. Configure AWS credentials on the Vagrant machine:
   ```sh
   aws configure
   ```
2. Jenkins user is created automatically during installation.
3. Add Jenkins user to the sudo group:
   ```sh
   sudo usermod -aG sudo jenkins
   ```

## Setting Up AWS Credentials for Jenkins (Vagrant VM)
1. Create the `.aws` directory inside the Jenkins home directory:
   ```sh
   sudo mkdir -p /var/lib/jenkins/.aws
   ```
2. Copy AWS credentials file from Vagrant machine:
   ```sh
   sudo cp /home/vagrant/.aws/credentials /var/lib/jenkins/.aws/credentials
   ```
3. Set the correct ownership and permissions:
   ```sh
   sudo chown -R jenkins:jenkins /var/lib/jenkins/.aws
   sudo chmod 600 /var/lib/jenkins/.aws/credentials
   ```

## Terraform Note
When running Terraform commands, using `cd myfolder` alone will return to the root directory. Instead, use:
In terraform 
```sh 
sh 'cd myfolder'
```
will not continue to the next line it will come back to the root directory. so:
1. Use sh """  ... """ for multiple commands like:
 ```sh 
sh """
        cd myfolder
        terraform init
        terraform plan
        terraform apply --auto-approve
    """
```
2. Or use
```sh
sh 'cd myfolder; terraform init; terraform plan; terraform apply --auto-approve'
 ```
3. Or Use:  
```sh
cd myfolder && terraform init && terraform plan && terraform apply --auto-approve
```
4. Alternatively, In fact best way is to use Jenkins `dir()` step:
```groovy
steps {
    dir('myfolder') {
        // Terraform commands here
    }
}
```
-----------------------------------------------------------------------
## Changing Jenkins Default Port (8080 → 8081)
### Step 1: Modify Configuration File
1. Open the Jenkins configuration file:
   ```sh
   sudo nano /etc/default/jenkins
   ```
2. Locate and update the port:
   ```sh
   JENKINS_PORT=8081
   ```
3. Save and exit (`Ctrl + X`, then `Y`, then `Enter`).
4. Restart Jenkins:
   ```sh
   sudo systemctl restart jenkins
   ```

### Step 2: Modify Systemd Service File
1. Open the Jenkins systemd service file:
   ```sh
   sudo nano /usr/lib/systemd/system/jenkins.service
   ```
2. Locate and update the environment variable:
   ```sh
   Environment="JENKINS_PORT=8081"
   ```
3. Save and exit (`Ctrl + X`, then `Y`, then `Enter`).
4. Reload systemd daemon:
   ```sh
   sudo systemctl daemon-reload
   ```
5. Restart Jenkins:
   ```sh
   sudo systemctl restart jenkins
   ```
6. Verify if another process is using port 8081:
   ```sh
   sudo netstat -tulnp | grep 8081
   ```

---


```
