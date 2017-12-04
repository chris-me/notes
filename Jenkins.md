## Docker

https://jenkins.io/doc/book/installing/#downloading-and-running-jenkins-in-docker

```bash
docker run --detach \
  --user root \
  --rm \
  --publish 8080:8080 \
  --volume jenkins-data:/var/jenkins_home \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --name jenkins \
  jenkinsci/blueocean
```

## Example Workflow

- Requirements: running Jenkins instance with default plugins installed
- Create new Job
- Name as wanted
- Choose 'Pipeline' --> OK
- Under 'Pipeline' choose 'Pipeline script from SCM' 
- Choose GIT, fill out repositori URL, credentials

Make sure this file is named 'Jenkinsfile' and checked in on the root of the repository thats tested / build:

```bash
pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment { 
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
```

