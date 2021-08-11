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
                        sh 'su - jenkins'
                        sh 'cd ${WORKSPACE}'
                        sh 'abuild checksum'
                        sh 'sudo abuild-keygen -a -i -n'
                        sh 'abuild -r'     
                    }
                }
            }    
        }  
    }
}

