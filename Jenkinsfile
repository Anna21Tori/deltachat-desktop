pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
		sh '/usr/bin/npm install'
		sh '/usr/bin/npm build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
		sh '/usr/bin/npm test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }

    post {
        failure {
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'anna21tori@gmail.com',
                subject: "Build failed in Jenkins ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
        success {
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'anna21tori@gmail.com',
                subject: "Successful build in Jenkins ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }
}
