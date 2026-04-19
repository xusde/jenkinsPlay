@Library('my-lib') _

pipeline {
    agent none

    triggers {
        githubPush()
    }

    stages {

        stage('Build & Test') {
            // runs on EVERY branch — always verify the code works
            agent { docker { image 'node:20-alpine' } }
            steps {
                echo 'Build & Test'
            }
            post {
                always {
                    echo 'Post of Build & Test Stage'
                }
            }
        }

        stage('Deploy to staging') {
            // only runs on the develop branch
            when { branch 'develop' }
            agent { docker { image 'node:20-alpine' } }
            steps {
                myDeploy('staging')
            }
        }

        stage('Deploy to production') {
            // only runs on main — and requires manual approval
            when { branch 'main' }
            agent { docker { image 'node:20-alpine' } }
            steps {
                myDeploy('production')
            }
        }


        stage('PR checks') {
        when {
            changeRequest()     // ONLY runs on PRs — never on real branches
        }
        steps {
            echo "Validating PR #${env.CHANGE_ID}"
            echo "Merging: ${env.CHANGE_BRANCH} → ${env.CHANGE_TARGET}"
        }
    }

    }

    post {
        success  { notifySlack('success') }
        failure  { notifySlack('failure') }
    }
}
