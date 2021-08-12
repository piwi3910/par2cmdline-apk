pipeline {
    environment{
        version = "0.8.1"
        build_user = "jenkins" 
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
                        sh 'adduser ${build_user} -G abuild -h /home/${build_user} -H -D'
                        sh 'chown -R ${build_user}:abuild /home/${build_user}'
                        sh 'echo "${build_user} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${build_user}'  
                    }
                }
            }    
        }
        stage('Build') {
            steps {
                container('builder') {
                    script {
                        sh 'sudo -u ${build_user} /bin/bash -c "cd ${WORKSPACE}"'
                        sh """sudo -u ${build_user} /bin/bash -c 'sed -i "s/__VERSION__/${version}/g" APKBUILD'"""    
                        sh 'sudo -u ${build_user} /bin/bash -c "abuild checksum"'
                        sh 'sudo -u ${build_user} /bin/bash -c "abuild-keygen -a -i -n"'
                        sh 'sudo -u ${build_user} /bin/bash -c "abuild -r"'
                    }
                    nexusArtifactUploader {
                        nexusVersion('nexus3')
                        protocol('https')
                        nexusUrl('repo.watteel.dev')
                        groupId('sp.sd')
                        version('3')
                        repository('apk')
                        credentialsId('705f4b11-7588-47f4-97ae-0834eec4dcd9')
                        artifact {
                            artifactId('par2cmdline')
                            type('apk')
                            classifier('debug')
                            file('/home/jenkins/packages/workspace/aarch64/par2cmdline-0.8.1-r1.apk')    
                        }    
                    }
                }
            }    
        }
    }
}
