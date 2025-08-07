pipeline {
    agent any
    stages {

        stage ('Clean up code') {
            steps {
                cleanWs()
            }

        }

        stage ('Build') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true 
                }
            }

            steps {

                    
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
