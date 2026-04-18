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
                channel: '#forum-group-project',       // override the default channel
                color: 'good',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
                message: """
                *Congrats! Build Succeeded!*
                Job: `${env.JOB_NAME}`
                Build: #${env.BUILD_NUMBER}
                Branch: `${env.GIT_BRANCH}`
                Duration: ${currentBuild.durationString}
                <${env.BUILD_URL}|View build logs>
                """
            )
        }
        failure { 
            echo 'Pipeline failed!'
            slackSend(
                channel: '#forum-group-project',       // override the default channel
                color: 'good',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
                message: """
                *Attention! Build Failed!*
                Job: `${env.JOB_NAME}`
                Build: #${env.BUILD_NUMBER}
                Branch: `${env.GIT_BRANCH}`
                Duration: ${currentBuild.durationString}
                <${env.BUILD_URL}|View build logs>
                """
            )
        }
    }
}
