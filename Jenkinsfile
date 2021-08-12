pipeline {
    environment{
        version ="0.8.1" 
    }
    
    agent {
        kubernetes {
        yamlFile 'buildpod.yaml'
        }
    }
    stages {
        stage('Prepare Build Env') {
            steps {
                container('builder') {
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
                container('builder') {
                    script {
                        sh 'sudo -u jenkins /bin/bash -c "cd ${WORKSPACE}"'
                        sh """sudo -u jenkins /bin/bash -c 'sed -i "s/__VERSION__/${version}/g" APKBUILD'"""    
                        sh 'sudo -u jenkins /bin/bash -c "abuild checksum"'
                        sh 'sudo -u jenkins /bin/bash -c "abuild-keygen -a -i -n"'
                        sh 'sudo -u jenkins /bin/bash -c "abuild -r"'
                    }
                }
            }    
        }  
    }
}

