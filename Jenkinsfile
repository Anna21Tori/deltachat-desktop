pipeline {
    agent {
        docker { image 'node:latest' }
	docker { image 'docker:dind' } 
    }
    
	
    stages {
        stage('Build') {
            steps {
                echo 'Building.......'
		checkout scm
		sh 'npm install'
		sh 'npm run build'
            }
        }
        stage('Test') {
	   when {
              	expression {currentBuild.result == null || currentBuild.result == 'SUCCESS'}
            }
            steps {
                echo 'Testing..........'
		sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying......'
		sh 'docker build -t delta-chat -f Dockerfile-deploy .'
            }
        }
    }

    post {
        failure {
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'anna21toria@gmail.com',
                subject: "Build failed in Jenkins ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
        success {
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'anna21toria@gmail.com',
                subject: "Successful build in Jenkins ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }
}
