# githubactions

steps to create Continuous Integration:
=======================================
1. create a workflow file called "github-actions-demo.yml" in the .github/workflows directory.
2. create a file .github and make a folder/directories workflows

github-actions-demo.yml:
========================
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
===
jobs:
======
Explore-GitHub-Actions:
=======================
runs-on: ubuntu-latest
steps:
======
- run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
- run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
- run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
- name: Check out repository code
uses: actions/checkout@v4
- run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
- run: echo "🖥️ The workflow is now ready to test your code on the runner."
- name: List files in the repository
run: |
ls ${{ github.workspace }}
- run: echo "🍏 This job's status is ${{ job.status }}."



steps:
======
1. name: name of the github actions
2. on: [push], developers merged the code into a  shared github repository, then this "on" event triggered the code then build,run and tests the code
3. jobs: define the tasks to run
4. uses: actions/checkout@v4 : checkout the github repo code.
5. on:
   workflow_call:, means used to create reusable workflows (means other workflows can call them, like expense-backend/expense-frontend)
6. A called workflow is a reusable workflow
7. caller workflow is the workflow that invokes (calls) the reusable workflow when certain events occur
   jobs:
   call-container-integration-reusable:
   uses: pdevops78/github-reusable-workflows/.github/workflows/ci.yml@main
8. for code review: use sonarqube
9. to pass code on sonarqube dashboard use "sonarscanner cli"

how to install sonarqube?
1. Install java17
2. curl -L -o sonarqube.zip https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.6.zip
3. https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.5.0.107428.zip
4. https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.5.1.90531.zip
5. unzip sonarqube.zip
6. sudo useradd sonar
   sudo chown -R sonar:sonar /opt/sonarqube
7. Start Sonarqube Service
============================
8. Step 1: Login as sonar user

sudo su - sonar
Step 2: Navigate to the start script directory.

cd /opt/sonarqube/bin/linux-x86-64
Step 3: Start the sonarqube service.
./sonar.sh start
Setting up Sonarqube as Systemd Service
=======================================
Step 1: Create a file /etc/systemd/system/sonarqube.service

sudo vi /etc/systemd/system/sonarqube.service
Step 2: Copy the following content onto the file.

[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=simple
User=sonarqube
Group=sonarqube
PermissionsStartOnly=true
ExecStart=/bin/nohup java -Xms32m -Xmx32m -Djava.net.preferIPv4Stack=true -jar /opt/sonarqube/lib/sonar-application-7.6.jar
StandardOutput=syslog
LimitNOFILE=65536
LimitNPROC=8192
TimeoutStartSec=5
Restart=always

[Install]
WantedBy=multi-user.target
Step 3: Start and enable sonarqube

sudo systemctl start sonarqube
sudo systemctl enable sonarqube
Step 4: Check the sonarqube status to ensure it is running as expected.

sudo systemctl status  sonarqube


Troubleshooting Sonarqube
=========================
All the logs of sonarqube are present in the /opt/sonarqube/logs directory.

cd /opt/sonarqube/logs
You can find the following log files.

es.log
sonar.log
web.log
access.log
Using the tail command you can check the latest logs. For example,

tail -f access.log

sonarScanner:
==============
1. Install Sonar scanner cli
   curl -L -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.1.0.4889-linux-x64.zip?_gl=1*w374vd*_gcl_au*MTQ0NzA4ODc2MS4xNzQ4MzMyNDU3*_ga*MTAyOTA5MTgwLjE3MTc1OTUzMzA.*_ga_9JZ0GZ5TC6*czE3NDgzMzI0NTYkbzgkZzEkdDE3NDgzMzM3MzckajUzJGwwJGgwJGQzc1Z5UjRsN2tuNjEtNkRVdlEyYS1ZR0xVQ000aW9VUEZR
2. uninstall zip file
3.vi /opt/sonar-scanner/conf/sonar-scanner.properties

once application  added in sonarqube 
1. if condition

Release Software:
==================
✅ Release branch → Used temporarily for preparing a version before merging.
✅ Trunk (main) → The final destination for stable code before deployment
Featured branch: Developers create their own repos in git
trunkbaseddevelopment.com, for branches

artifactory to deploy a file:
=============================
$ curl -v --user username:password --data-binary@artifactzip -X PUT
"http://localhost:8080/artifactory/expense-backend/1.0.0.zip"

 on:
  push:
   tags:
    - "*"
 
** pass info through expense backend : release_archive_files: "*.js packahe.json schema VERSION"
echo ${GITHUB_REF_NAME}>VERSION , means GITHUB_REF_NAME=test4 or 1.0.0 redirect version replace with previous data

run: zip -r artifcat.zip ${{input.release_archive_files}}