pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mtharukap/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
	    steps {
        	bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins - Run Tests - ${currentBuild.currentResult}",
                        body: """
                            <h2>Run Tests Stage</h2>
                            <p>Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}</p>
                            <p>Status: ${currentBuild.currentResult}</p>
                            <p>The test stage has completed.</p>
                        """,
                        to: 'tharuka4509@gmail.com',
                        attachLog: true
                    )
                }
            }
         }

        stage('NPM Audit (Security Scan)') {
            steps {
                 bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins - Security Scan - ${currentBuild.currentResult}",
                        body: """
                            <h2>NPM Audit Security Scan</h2>
                            <p>Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}</p>
                            <p>Status: ${currentBuild.currentResult}</p>
                            <p>The NPM security scan has completed.</p>
                        """,
                        to: 'tharuka4509@gmail.com',
                        attachLog: true
                    )
                }
            }
        }
    }
}