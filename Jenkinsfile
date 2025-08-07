pipeline {
    agent any
    stages {
        stage ('Build') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true 
                }
            }

            steps {

                    cleanWs()
                

                
                sh '''
                    ls -l
                    node --version
                    npm --version
                    npm install
                    npm run build
                    ls -l
                '''    
                }
               
            }
        }           
    }
}
