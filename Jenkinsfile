pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix://\$(pwd)/docker.sock"
        STAGE_INSTANCE = 'ubuntu@13.51.146.196'
    }
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {
                    sh "ssh -i /var/lib/jenkins/jen-stage -o StrictHostKeyChecking=no -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ubuntu@3.8.126.107 & echo \$! > /tmp/tunnel.pid"

                    sleep 5
                }
            }
        }
    }
        stage('Deploy') {
            steps {
                script {
                    sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
                }
            }
        }
}
    post {
        always {
            script {
                sh 'pkill -F /tmp/tunnel.pid & rm /var/lib/jenkins/workspace/Staging/docker.sock'
            }
        }
    }
