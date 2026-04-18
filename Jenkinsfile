pipeline {
    agent any                        // run on any available agent

    stages {
        stage('Checkout') {
            steps {
                checkout scm         // pull code from Git
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building..."'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Testing..."'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'        // only deploy from main branch
            }
            steps {
                sh 'echo "Deploying to staging..."'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!' 
            slackSend(
                channel: '#deployments',       // override the default channel
                color: 'good',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
                message: 'Build passed!'
            )
        }
        failure { 
            echo 'Pipeline failed!'
            slackSend(
                channel: '#deployments',       // override the default channel
                color: 'good',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
                message: 'Build failed!'
            )
        }
    }
}
