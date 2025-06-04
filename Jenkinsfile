pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stages {
        stage('Test') {
            steps {
                sh "go test ./..."
            }
        }
        stage('Build') {
            steps {
                sh "go build main.go"
            }
        }
         stage('Docker Build') {
            steps {
                sh "docker build . --tag ttl.sh/myapp:2h"
                sh "docker push ttl.sh/myapp:2h"
            }
        }
        stage('Deploy to ec2') {
           steps {
                sshagent(['myappkey']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@54.229.245.200 'docker pull ttl.sh/myapp:2h && docker run -d --name myapp -p 4444:4444 ttl.sh/myapp:2h'
                    '''
                }   
            }
        }
    }
}
