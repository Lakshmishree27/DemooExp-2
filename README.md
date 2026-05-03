You will:

Install tools
Setup Jenkins
Build project
Deploy .jar
(Optional) Use Ansible
STEP 0 — Install Required Tools

Install these first:

Java JDK
Apache Maven
Git
Jenkins

Verify in CMD:

java -version
mvn -version
git --version
STEP 1 — Start Jenkins

Open browser:

http://localhost:8080

Login → Dashboard opens

STEP 2 — Create Project

In Jenkins:

Click New Item
Name:
HelloMaven-CI
Select Freestyle Project
Click OK
STEP 3 — Connect GitHub
Source Code Management → Git
Paste:
https://github.com/Lakshmishree27/hello-maven.git
Branch:

*/main
STEP 4 — Add Build Steps
Build → Add build step → Execute Windows batch command
Step 1:
mvn clean package
🔹 Step 2:
mkdir C:\Users\%USERNAME%\.jenkins\deployment1 2>nul
copy target\*.jar C:\Users\%USERNAME%\.jenkins\deployment1\
STEP 5 — Archive Artifact
Post-build → Archive the artifacts
target/*.jar
Click:
Apply
Save
STEP 6 — Run Project
Click Build Now
STEP 7 — Check Output
Go to:
C:\Users\YOUR_USERNAME\.jenkins\deployment1
You will see:
hello-maven-1.0-SNAPSHOT.jar
DONE (without Ansible)

OPTIONAL — WITH ANSIBLE (FOR FULL MARKS)
 STEP 8 — Install Ansible (WSL)

Open PowerShell (Admin):

wsl --install

Restart → Open Ubuntu

Then:

sudo apt update
sudo apt install ansible -y

Check:

ansible --version
STEP 9 — Create Ansible Files
hosts.ini
[local]
localhost ansible_connection=local
deploy.yml
---
- name: Deploy jar
  hosts: local
  tasks:

    - name: Create folder
      file:
        path: "/mnt/c/Users/YOUR_USERNAME/.jenkins/deployment"
        state: directory

    - name: Copy jar
      copy:
        src: "/mnt/c/Users/YOUR_USERNAME/.jenkins/workspace/HelloMaven-CI/target/hello-maven-1.0-SNAPSHOT.jar"
        dest: "/mnt/c/Users/YOUR_USERNAME/.jenkins/deployment/HelloMaven.jar"
        remote_src: yes
🔥 STEP 10 — Run Ansible
ansible-playbook -i hosts.ini deploy.yml
✅ FINAL OUTPUT (Ansible)

Check:

C:\Users\YOUR_USERNAME\.jenkins\deployment

✔ .jar file present
