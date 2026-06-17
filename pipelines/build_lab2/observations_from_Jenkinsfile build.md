# Jenkins Environment Variables - Learning Notes

## Objective

Learn how to define custom environment variables and use Jenkins built-in environment variables inside a pipeline.

---

# Jenkinsfile Used

```groovy
pipeline {

    agent any

    environment {
        APP_NAME = "jenkins-learning"
        ENV_NAME = "dev"
        VERSION = "1.0.0"
    }

    stages {

        stage('Print Variables') {
            steps {
                sh 'echo APP_NAME=$APP_NAME'
                sh 'echo ENV_NAME=$ENV_NAME'
                sh 'echo VERSION=$VERSION'
            }
        }

        stage('Jenkins Variables') {
            steps {
                sh 'echo BUILD_NUMBER=$BUILD_NUMBER'
                sh 'echo JOB_NAME=$JOB_NAME'
                sh 'echo WORKSPACE=$WORKSPACE'
                sh 'echo BUILD_URL=$BUILD_URL'
            }
        }

    }
}
```
# Console Output

```text
Started by user sai nikhil

Obtained pipelines/build_lab2/Jenkinsfile from git https://github.com/gsainik/jenkins-handbook.git

[Pipeline] Start of Pipeline

[Pipeline] node

Running on Jenkins in /var/jenkins_home/workspace/pipeline-scm

+ echo APP_NAME=jenkins-learning

APP_NAME=jenkins-learning

[Pipeline] sh

+ echo ENV_NAME=dev

ENV_NAME=dev

[Pipeline] sh

+ echo VERSION=1.0.0

VERSION=1.0.0

+ echo BUILD_NUMBER=7

BUILD_NUMBER=7

[Pipeline] sh

+ echo JOB_NAME=pipeline-scm

JOB_NAME=pipeline-scm

[Pipeline] sh

+ echo WORKSPACE=/var/jenkins_home/workspace/pipeline-scm

WORKSPACE=/var/jenkins_home/workspace/pipeline-scm

[Pipeline] sh

+ echo BUILD_URL=http://localhost:8080/job/pipeline-scm/7/

BUILD_URL=http://localhost:8080/job/pipeline-scm/7/

[Pipeline] End of Pipeline

Finished: SUCCESS
```
---

# Pipeline Execution Flow

```text
GitHub Repository
       ↓
Jenkins Clones Repository
       ↓
Reads Jenkinsfile
       ↓
Loads Environment Variables
       ↓
Executes Pipeline Stages
       ↓
Prints Variables
```

---

# SCM Checkout Process

During build execution Jenkins performed:

```text
Obtained Jenkinsfile from git
```

Meaning:

```text
GitHub Repository
       ↓
Jenkins Clones Repository
       ↓
Reads Jenkinsfile
       ↓
Executes Pipeline
```

Repository:

```text
https://github.com/gsainik/jenkins-handbook.git
```

Branch:

```text
main
```

Commit:

```text
134e1bcf660d05bcddbb252fb82a7609d5291f90
```

Commit Message:

```text
path refactor
```

---

# Custom Environment Variables

Defined in:

```groovy
environment {
    APP_NAME = "jenkins-learning"
    ENV_NAME = "dev"
    VERSION = "1.0.0"
}
```

Output:

```text
APP_NAME=jenkins-learning
ENV_NAME=dev
VERSION=1.0.0
```

Learning:

```text
Custom variables are defined by the user inside the environment block.
```

Used for:

* Application Name
* Environment Name
* Version Numbers
* URLs
* Docker Image Names
* Configuration Values

---

# Jenkins Built-in Variables

Jenkins automatically provides environment variables during build execution.

---

## BUILD_NUMBER

Output:

```text
BUILD_NUMBER=7
```

Meaning:

```text
Current build execution number.
```

Example:

```text
Build #1
Build #2
Build #3
...
Build #7
```

---

## JOB_NAME

Output:

```text
JOB_NAME=pipeline-scm
```

Meaning:

```text
Current Jenkins Job Name.
```

---

## WORKSPACE

Output:

```text
WORKSPACE=/var/jenkins_home/workspace/pipeline-scm
```

Meaning:

```text
Directory where Jenkins cloned the repository.
```

Learning:

```text
Every Jenkins job gets its own workspace.
```

Example:

```text
/var/jenkins_home/workspace/pipeline-scm
```

---

## BUILD_URL

Output:

```text
BUILD_URL=http://localhost:8080/job/pipeline-scm/7/
```

Meaning:

```text
Direct URL to the build execution.
```

Useful for:

* Notifications
* Emails
* Slack Messages
* Build Reports

---

# Understanding withEnv

Console Output:

```text
[Pipeline] withEnv
```

Meaning:

```text
Jenkins exports environment variables before executing shell commands.
```

Flow:

```text
Environment Block
        ↓
withEnv
        ↓
Shell Commands
```

This is why:

```bash
echo $APP_NAME
```

works inside:

```groovy
sh 'echo $APP_NAME'
```

---

# Interview Questions

## Q1. What is an Environment Variable?

Answer:

```text
A key-value pair available during pipeline execution.
```

Example:

```groovy
environment {
    APP_NAME = "jenkins-learning"
}
```

---

## Q2. Difference Between Custom and Built-in Variables?

Custom Variables:

```text
APP_NAME
ENV_NAME
VERSION
```

Defined by developers.

Built-in Variables:

```text
BUILD_NUMBER
JOB_NAME
WORKSPACE
BUILD_URL
```

Provided automatically by Jenkins.

---

## Q3. How Does Jenkins Make Variables Available to Shell Commands?

Answer:

```text
Jenkins uses withEnv to export variables into the shell environment before executing commands.
```

Example:

```groovy
sh 'echo $BUILD_NUMBER'
```

---

## Q4. What is WORKSPACE?

Answer:

```text
The directory where Jenkins checks out source code and executes builds.
```

Example:

```text
/var/jenkins_home/workspace/pipeline-scm
```

---

## Q5. What is BUILD_NUMBER?

Answer:

```text
A unique number assigned to each build execution.
```

Example:

```text
Build #7
```

---

# Key Takeaways

```text
1. Jenkins supports custom environment variables.
2. Jenkins automatically provides built-in variables.
3. Environment variables are accessible inside shell commands.
4. withEnv is used internally by Jenkins to export variables.
5. WORKSPACE contains the cloned repository.
6. BUILD_NUMBER uniquely identifies a build.
7. BUILD_URL points to the current build execution.
8. JOB_NAME identifies the current Jenkins job.
```
