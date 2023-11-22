/* groovylint-disable-next-line CompileStatic */
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
                    /* sh 'whoami' */
                    /* groovylint-disable-next-line LineLength */
                    /* sh "ssh -i /var/lib/jenkins/id_rsa -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid" */
                    // Иногда не достаточно времени для создания туннеля, добавим паузу
                    /* sh 'cat /tmp/tunnel.pid' */
                    /* sleep 15 */

                    /* groovylint-disable-next-line LineLength */
                    sh "ssh -i /var/lib/jenkins/id_rsa -o StrictHostKeyChecking=no -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ubuntu@13.51.146.196 & echo \$! > /tmp/tunnel.pid"

                    sleep 5
                }
            }
            stage('Deploy') {
                steps {
                    /* groovylint-disable-next-line NestedBlockDepth */
                    script {
                        sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
                    }
                }
            }
        }
        post {
            always {
                script {
                    sh 'pkill -F /tmp/tunnel.pid & rm /var/lib/jenkins/workspace/Stagind/docker.sock'
                }
            }
        }
    }
}
