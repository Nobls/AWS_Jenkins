pipeline {
    agent any
    
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {
                    /* groovylint-disable-next-line LineLength */
                    sh "ssh -i /var/lib/jenkins/.ssh/id_rsa -o StrictHostKeyChecking=no -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ubuntu@51.20.252.66 & echo \$! > /tmp/tunnel.pid"

                    sleep 10
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'DOCKER_HOST=unix://\$(pwd)/docker.sock docker ps -a'
                }
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
