pipeline {
    agent any
    environment {
        /* DOCKER_HOST = "unix://\$(pwd)/docker.sock" */
        STAGE_INSTANCE = 'ubuntu@51.20.142.56'
    }
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {
                    /* groovylint-disable-next-line LineLength */
                    sh "ssh -i /var/lib/jenkins/jenkins_key -o StrictHostKeyChecking=no -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"

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
                sh 'pkill -F /tmp/tunnel.pid'
            }
        }
    }
