# Jenkins Shell Commands Learning Notes

## Objective

Understand where Jenkins executes commands, where source code is cloned, and how Jenkins agents work.

---

# Jenkinsfile Used

```groovy
pipeline {

    agent any

    stages {

        stage('Workspace Info') {
            steps {
                sh 'pwd'
                sh 'whoami'
                sh 'hostname'
            }
        }

        stage('List Files') {
            steps {
                sh 'ls -ltr'
            }
        }

    }
}
```
Flow:

```text
Jenkins clones code into a workspace
        ↓
Executes shell commands on an agent/node
        ↓
In our setup, the agent is the Jenkins Docker container
```

---

# Below is the console output


```text
Started by user sai nikhil
Obtained pipelines/Jenkinsfile from git https://github.com/gsainik/jenkins-handbook.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/jenkins_home/workspace/pipeline-scm

+ pwd
/var/jenkins_home/workspace/pipeline-scm
[Pipeline] sh
+ whoami
jenkins
[Pipeline] sh
+ hostname
b4a8edc1412f
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (List Files)
[Pipeline] sh
+ ls -ltr
total 256
-rw-r--r-- 1 jenkins jenkins  1902 Jun 16 17:23 2.jenkins Pipelines.md
-rw-r--r-- 1 jenkins jenkins  9172 Jun 16 17:23 1.jenkins Install and setup.md
-rw-r--r-- 1 jenkins jenkins 98034 Jun 16 17:23 image.png
-rw-r--r-- 1 jenkins jenkins 31750 Jun 16 17:23 image-3.png
-rw-r--r-- 1 jenkins jenkins 46384 Jun 16 17:23 image-2.png
-rw-r--r-- 1 jenkins jenkins 61015 Jun 16 17:23 image-1.png
drwxr-xr-x 2 jenkins jenkins  4096 Jun 16 17:51 pipelines
```

# Key Points to Analyze
## Command 1 : pwd

```bash
pwd
```

Question:

What path is shown?

Example:

```
/var/jenkins_home/workspace/pipeline-scm
```
This teaches: ```Jenkins Workspace```

# Command 2: whoami

```bash
whoami
```

Question: Which user executed the command?

Usually: ```jenkins```

This teaches: ```Pipeline runs as Jenkins user```

# Command 3: hostname

```bash
hostname
```

Question: What hostname is shown?

In Docker it will usually be: container-id

Example: ```9f2a4b6c8d7e```

This teaches:

```Pipeline is running inside the Jenkins container```

# Command 4: ls -ltr
```bash
ls -ltr
```

Question: Which files are listed?

You should see:

```text
-rw-r--r-- 1 jenkins jenkins  1902 Jun 16 17:23 2.jenkins Pipelines.md
-rw-r--r-- 1 jenkins jenkins  9172 Jun 16 17:23 1.jenkins Install and setup.md
-rw-r--r-- 1 jenkins jenkins 98034 Jun 16 17:23 image.png
-rw-r--r-- 1 jenkins jenkins 31750 Jun 16 17:23 image-3.png
-rw-r--r-- 1 jenkins jenkins 46384 Jun 16 17:23 image-2.png
-rw-r--r-- 1 jenkins jenkins 61015 Jun 16 17:23 image-1.png
drwxr-xr-x 2 jenkins jenkins  4096 Jun 16 17:51 pipelines
```


This teaches:

```
Jenkins cloned the Git repository into the workspace
```

# Important Concepts Learned

## Jenkins Workspace

Definition:

```text
A directory where Jenkins checks out source code and executes builds.
```

Example:

```text
/var/jenkins_home/workspace/pipeline-from-scm
```

---

## Jenkins Agent

Definition:

```text
The machine/node where the pipeline executes.
```

Current Lab Setup:

```text
Windows
   ↓
WSL
   ↓
Docker
   ↓
Jenkins Container
   ↓
Pipeline Execution
```

---

## Pipeline Execution Flow

```text
Git Repository
      ↓
Jenkins Checkout
      ↓
Workspace Created
      ↓
Execute Shell Commands
      ↓
Store Console Output
```

---

# Interview Questions

## Q1. Where does Jenkins clone the repository?

Answer:

```text
Inside the Jenkins workspace.
```

Example:

```text
/var/jenkins_home/workspace/<job-name>
```

---

## Q2. Where are shell commands executed?

Answer:

```text
On the Jenkins agent/node running the pipeline.
```

Current Lab:

```text
Jenkins Docker Container
```

---

## Q3. What does "agent any" mean?

Pipeline:

```groovy
agent any
```

Answer:

```text
Run the pipeline on any available Jenkins node/agent.
```

Current Lab:

```text
Built-in Jenkins node running inside the Jenkins container.
```

---

# Key Takeaways

```text
1. Jenkins clones code into a workspace.
2. Shell commands execute on the Jenkins agent.
3. In our setup, the agent is the Jenkins Docker container.
4. pwd shows the workspace location.
5. whoami shows the user executing the build.
6. hostname shows the machine/container running the build.
7. ls -ltr verifies that Jenkins checked out the repository.
```