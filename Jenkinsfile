/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix://\$(pwd)/docker.sock"
        STAGE_INSTANCE = 'ubuntu@16.171.170.50'
    }
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {
                    /* groovylint-disable-next-line LineLength */
                    sh "ssh -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
                    sleep 5
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
                sh 'pkill -F /tmp/tunnel.pid'
            }
        }
    }
}
