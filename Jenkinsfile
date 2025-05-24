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
                sshagent(['pk-test']) {
               sh '''
                  ls -l main
                  ssh -o StrictHostKeyChecking=no laborant@target 'pwd; ls -la ~'
                '''
                sh 'scp -o StrictHostKeyChecking=no main laborant@target:~'
                sh 'ssh -o StrictHostKeyChecking=no laborant@target "chmod +x ~/main && sudo systemctl daemon-reload && sudo systemctl restart main.service"'
                }   
            }
        }
    }
}
