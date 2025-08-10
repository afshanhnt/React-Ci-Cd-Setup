pipeline {
    agent {
        docker {
            image 'node:22.11.0-alpine3.20'
            args "-u root -v ${WORKSPACE}/.npm-cache:/root/.npm"
            reuseNode true
        }
    }

    environment {
        NODE_ENV = 'test'
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
    }

    options {
        skipDefaultCheckout(true)
        durabilityHint('PERFORMANCE_OPTIMIZED')
    }

    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                checkout scm
            }
        }

        stage('Install & Build') {
            steps {
                sh '''
                    echo "Node: $(node --version)"
                    echo "NPM: $(npm --version)"
                    npm ci --prefer-offline --cache /root/.npm
                    npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    npm install -g vercel
                    vercel --prod --token=$VERCEL_TOKEN --confirm --name=cicdproject
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
