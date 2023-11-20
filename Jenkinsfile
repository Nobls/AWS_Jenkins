/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix:/\$(pwd)/docker.sock"
        STAGE_INSTANCE = 'ubuntu@16.171.170.50'
    }
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {
                    /* groovylint-disable-next-line LineLength */
                    sh "ssh -i /var/lib/jenkins/id_rsa -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
                    sleep 15
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "${DOCKER_HOST}"
                    sh "ls -la /var/lib/jenkins/workspace/AWS/"
                    sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
                    sh "echo 111"
                }
            }
        }
    }
    post {
        always {
            script {
                sh "rm /var/lib/jenkins/workspace/AWS/docker.sock"
                sh "pkill -F /tmp/tunnel.pid" & "rm /tmp/tunnel.pid"
            }
        }
    }
}
