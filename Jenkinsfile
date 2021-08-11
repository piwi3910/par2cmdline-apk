pipeline {
    agent {
        kubernetes {
        yamlFile 'buildpod.yaml'
        }
    }
    stages {
        stage('Prepare Build Env') {
            steps {
                container('alpine-base') {
                    script {
                        sh 'apk add alpine-sdk automake autoconf sudo'
                        sh 'adduser jenkins -G abuild -h /home/jenkins -H -D'
                        sh 'chown -R jenkins:abuild /home/jenkins'
                        sh 'echo "jenkins ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/jenkins'  
                    }
                }
            }    
        }
        stage('Build') {
            steps {
                container('alpine-base') {
                    script {
                        sh 'sudo -u jenkins /bin/bash -c "cd ${WORKSPACE}"'
                        sh 'sudo -u jenkins /bin/bash -c "abuild checksum"'
                        sh 'sudo -u jenkins /bin/bash -c "sudo abuild-keygen -a -i -n"'
                        sh 'sudo -u jenkins /bin/bash -c "abuild -r"'
                    }
                }
            }    
        }  
    }
}

