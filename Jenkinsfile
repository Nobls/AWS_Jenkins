pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix:/\$(pwd)/docker.sock"
        STAGE_INSTANCE = 'ubuntu@13.51.205.191'
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

                    withCredentials([sshUserPrivateKey(credentialsId: '022e60cf-c9a9-4c05-a898-7055d1f4fa25', keyFileVariable: 'SSH_KEY')]) {
                        sh 'whoami'
                        // Используйте переменную секретного ключа в команде ssh
                        sh "ssh -i ${SSH_KEY} -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
                        // Добавим паузу, если это необходимо
                        sleep 15
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "${DOCKER_HOST}"
                    sh 'ls -la /var/lib/jenkins/workspace/Staging/'
                    sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
                    sh 'echo 111'
                }
            }
        }
    }
    post {
        always {
            script {
               /*  sh 'rm /var/lib/jenkins/workspace/AWS/docker.sock' */
               /*  sh 'pkill -F /tmp/tunnel.pid' & 'rm /tmp/tunnel.pid' */

                withCredentials([sshUserPrivateKey(credentialsId: 'your-ssh-credentials-id', keyFileVariable: 'SSH_KEY')]) {
                    sh 'rm /var/lib/jenkins/workspace/Staging/docker.sock'
                    sh 'pkill -F /tmp/tunnel.pid' // Внимание: 'rm /tmp/tunnel.pid' было удалено, так как у вас в коде используется & между командами
                }
            }
        }
    }
}