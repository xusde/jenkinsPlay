pipeline {
    agent any // run on any available agent

    triggers {
        githubPush()
        cron('H 5 * * *')
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
            agent { docker { image 'node:20-alpine' } }
            steps {
                sh 'npm --version'
                echo 'npm build'
            }
        }

        stage('Test') {
            parallel {
                stage('Test on usw2') {
                    agent { docker { image ('node:20-alpine') } }
                    steps {
                        echo 'test on usw2'
                        sh 'touch pass.txt'
                        sh 'echo "hello" >> pass.txt'
                        stash name: 'pass', includes: 'pass.txt'
                    }
                }

                stage('Test on use2') {
                    agent { docker { image ('node:20-alpine') } }
                    steps {
                        echo 'test on use2'
            
                    }
                }
            }
        }


        stage('check unstash') {
            steps {
                sh 'echo "checking unstashing..."'
                unstash name: 'pass'
                sh 'cat pass.txt'
            }
        }
        
        
        stage('Deploy') {
            when {
                branch 'main'        // only deploy from main branch
            }
            steps {
                sh 'echo "Deploying to staging..."'
                unstash name: 'pass'
                sh 'cat pass.txt'
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
                color: 'danger',                 // 'good' (green), 'warning' (yellow), 'danger' (red), or a hex like '#0000FF'
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
