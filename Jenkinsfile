@Library('my-lib') _     // the underscore is required — it imports the library into scope

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // call your shared function with named options
                myBuild(nodeVersion: '20', runLint: true)
            }
        }

        stage('Deploy staging') {
            steps {
                myDeploy('staging')
            }
        }

        stage('Deploy production') {
            when { branch 'main' }
            steps {
                myDeploy('production')
            }
        }
    }

    post {
        success  { notifySlack('success') }
        failure  { notifySlack('failure') }
        unstable { notifySlack('unstable') }
    }
}
