pipeline {
    agent any                        // run on any available agent

    trigger {
        githubPush()
        cron(H/1 * * * *)
    }
    
    environment {
        SLACK_USER_PXU = 'U08FE9PGHU6'
        SLACK_CHANNEL = '#jenkins-slack'
    }

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
                channel: env.SLACK_CHANNEL,       // override the default channel
                color: 'good',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
                message: """
                <@${env.SLACK_USER_PXU}>
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
                channel: env.SLACK_CHANNEL,       // override the default channel
                color: 'good',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
                message: """
                <@${env.SLACK_USER_PXU}>
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
