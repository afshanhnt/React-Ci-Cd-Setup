pipeline {
    agent any

    environment {
        NODE_ENV = 'test'
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
    }


    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Clean up code') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout using SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:22.12.0-alpine3.20'
                    args "-u root -v ${WORKSPACE}/.npm-cache:/root/.npm"
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -l
                    node --version
                    npm --version
                    npm ci --prefer-offline --cache /root/.npm
                    npm run build
                    ls -l
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:22.12.0-alpine3.20'
                    args '-u root'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm run test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g vercel
                    echo $MY_VAR
                    vercel --prod --token=$VERCEL_TOKEN --confirm --name=cicdproject
                '''
            }
        }
    }
}
