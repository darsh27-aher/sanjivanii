# DevOps Practical

## Things to Check Before Proceeding

### Check That Git Is Installed in the System
1. **Verify Installation**: 
   ```bash
   git --version
   ```
   If not installed, install Git:
   ```bash
   sudo apt update
   sudo apt install git -y
   ```

2. **Verify Java Installation**:
   ```bash
   java --version
   ```
   - Java version should be 8, 11, or 21.
   - If not installed, install Java:
     ```bash
     sudo apt update
     sudo apt install openjdk-11-jdk -y
     ```
   - Set the `JAVA_HOME` environment variable (if needed):
     ```bash
     echo "export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))" >> ~/.bashrc
     source ~/.bashrc
     ```

---

## Step 1: Setup Git Repository

1. **Create a Folder**: Save the Java code in the folder.
2. **Initialize Git**: Open the terminal in the folder and run:
   ```bash
   git init
   ```
3. **Stage Java Files**:
   ```bash
   git add .
   ```
4. **Commit Changes**:
   ```bash
   git commit -m "<any message>"
   ```
5. **Link to Remote Repository**:
   - Create a repository on [GitHub](https://www.github.com).
   - Copy the repository link and run:
     ```bash
     git remote add origin <paste link here>
     ```
6. **Push to GitHub**:
   ```bash
   git push -u origin master
   ```
   Or:
   ```bash
   git push -u origin main
   ```
**If any error occurs, use these commands:**
   ```bash
   echo "HELLO" > README.md
   git add .
   git commit -m "<your message>"
   git push -u origin main
   ```

---

## Step 2: Download and Setup Jenkins

1. **Install Jenkins**:
   ```bash
   sudo apt update
   sudo apt install -y openjdk-11-jdk wget gnupg
   wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt install -y jenkins
   ```

2. **Start Jenkins**:
   ```bash
   sudo systemctl start jenkins
   ```

3. **Enable Jenkins to Start on Boot**:
   ```bash
   sudo systemctl enable jenkins
   ```

4. **Access Jenkins**:
   - Open your browser and navigate to: [http://localhost:8080](http://localhost:8080)

5. **Retrieve Initial Admin Password**:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

---

## Step 3: Create a Jenkins Job

1. **Open Jenkins**:
   [http://localhost:8080](http://localhost:8080)
2. **Create a New Job**:
   - Click on "New Item" in the dashboard.
   - Enter a job name.
   - Select "Freestyle Project" and click OK.
3. **Configure the Job**:
   - Go to the "Source Code Management" section.
   - Select "Git".
   - Enter the GitHub repository URL.
   - Choose the branch (main or master).
4. **Build Trigger**:
   - Check "Poll SCM" to let Jenkins check for changes.
   - In the schedule field, enter:
     ```bash
     H/5 * * * *
     ```
5. **Build Steps**:
   - In the "Build" section, add a step:
     - Select "Execute Shell".
   - Enter commands to compile and run Java code:
     ```bash
     javac <program_name>.java
     java <program_name>
     ```
6. **Save and Build the Job**:
   - Click "Save".
   - Click "Build Now".
7. **Verify the Build Output**:
   - Go to "Build History".
   - Click on the build number and view the console output.

---

## Step 4: Automate the Build (Optional cause without this it also work but still do!!)
1. **Configure Webhook in GitHub**:
   - Go to the repository settings.
   - Navigate to "Webhooks" and click "Add webhook".
   - Set the Payload URL to:
     ```
     http://<your-server-ip>:8080/github-webhook/
     ```
   - Select "application/json" as content type.
   - Choose "Just the push event".

2. **Install GitHub Integration Plugin in Jenkins**:
   - Go to "Manage Jenkins > Manage Plugins".
   - Search for "GitHub Integration Plugin" and install it.

3. **Update Jenkins Job Configuration**:
   - Enable the "GitHub hook trigger for GITScm polling" option in the job configuration.

4. **Test Automation**:
   - Push changes to the GitHub repository.
   - Verify the new build is triggered in Jenkins.
   - Check the console output for successful build and execution.

---

## Step 5: Deploy Application Locally

1. **Create a Deployment Script**:
   - For Ubuntu (`deploy.sh`):
     ```bash
     #!/bin/bash
     echo "Starting the HelloWorld application..."
     java <program_name>
     ```

2. **Test the Script**:
   - Make it executable:
     ```bash
     chmod +x deploy.sh
     ```
   - Run the script:
     ```bash
     ./deploy.sh
     ```

3. **Add the Script to Jenkins**:
   - In Jenkins, navigate to the job configuration.
   - Add a post-build step to execute the script:
     ```bash
     ./deploy.sh
     ```

---

## Step 6: Test the Pipeline (CI/CD)

1. **Make Changes to the Code**:
   - Edit the Java file.
   - Save the changes.

2. **Push Changes to the Repository**:
   ```bash
   git add <filename>
   git commit -m "Updated message"
   git push origin master
   ```

3. **Observe Jenkins Automation**:
   - Verify a new build is triggered automatically.
   - Check the console output for successful build and deployment.

4. **Verify the Application**:
   - Test the application locally.
   - For web services, test the local URL.

5. **Troubleshoot Issues**:
   - Check Jenkins console output for errors.
   - Verify environment setup and configurations.
