pipeline {
    agent {
        kubernetes {
        yamlFile 'buildpod.yaml'
        }
    }
    stages {
        stage('Check Github releases') {
            steps {
                container('alpine-base') {
                    script {
                        sh 'echo "hello"'
                        sh 'whoami'
                        sh 'pwd'  
                    }
                }
            }    
        } 
    }
}

