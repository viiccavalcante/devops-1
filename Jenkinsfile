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
                withCredentials([sshUserPrivateKey(
                    credentialsId:'target-ssh-key',
                    keyFileVariable:'ssh_key',
                    usernameVariable: 'ssh_user')]){
                sh """
                    scp main ${ssh_user}target: -i ${ssh_key} -o StrictHostKeyChecking=no
                    """
                    }
                    
            }
        }
    }
}
