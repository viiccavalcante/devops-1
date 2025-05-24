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

        stage('Deploy') {
            steps {
                sshagent(['9be808c2-50c0-497d-bf4e-d5dcc6a20b7d4']) {
                sh 'scp -o StrictHostKeyChecking=no main laborant@target:~'
                sh 'ssh -o StrictHostKeyChecking=no laborant@target "chmod +x ~/main && sudo systemctl daemon-reload && sudo systemctl restart main.service"'
                }   
            }
        }
    }
}
