## Docker

https://jenkins.io/doc/book/installing/#downloading-and-running-jenkins-in-docker

Docker start script:

```bash
docker run --detach \
  --user root \
  --restart=unless-stopped \
  --publish 10085:8080 \
  --volume /docker-volumes/jenkins-data:/var/jenkins_home \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --name jenkins \
  jenkinsci/blueocean
```

Apache config:

```apache
<VirtualHost *:80>
    ServerName jenkins.example.com
    ProxyPass / http://localhost:10085/
    ProxyPassReverse / http://localhost:10085/
</VirtualHost>
```

## Example Workflow

https://jenkins.io/doc/tutorials/building-a-node-js-and-react-app-with-npm/

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

