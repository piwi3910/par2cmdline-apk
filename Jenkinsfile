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
                        sh """sudo -u jenkins /bin/bash -c "whoami"
                        whoami
                        cd ${WORKSPACE}
                        abuild checksum
                        sudo abuild-keygen -a -i -n
                        abuild -r"""     
                    }
                }
            }    
        }  
    }
}

