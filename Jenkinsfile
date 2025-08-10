pipeline {
    agent any
   
    stages {
        stage ('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    
                    reuseNode true 
                }
            }
        

            steps {

                sh '''
                    ls -l
                    node --version
                    npm --version
                    apk add --no-cache python3 make g++
                    npm install
                    npm run build
                    ls -l
                '''    
            }
        }           
    }
}
