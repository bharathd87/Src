pipeline {
    agent any

    environment {
        GIT_REPO  = 'https://github.com/bharathd87/cdevops.git'
        BRANCH    = 'main'
        RECIPIENT = 'bharath.hyr96@gmail.com'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace'
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                echo "Cloning ${GIT_REPO} branch ${BRANCH}"
                git branch: "${BRANCH}", url: "${GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                sh 'dos2unix build.sh || true'
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }
    }

    post {
        always {
            echo 'Archiving build artifacts...'
            archiveArtifacts artifacts: 'build/myfirmware.*', fingerprint: true
        }

        success {
            echo 'Build succeeded! Sending email...'
            emailext(
                subject: "Build Success: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
                body: "<p>Build succeeded in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>",
                to: "${RECIPIENT}"
                // Optional: from can be set here if needed, otherwise use global SMTP from Manage Jenkins
            )
        }

        unstable {
            echo 'Build unstable. Sending email...'
            emailext(
                subject: "Build Unstable: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
                body: "<p>Build is <b>UNSTABLE</b> in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>",
                to: "${RECIPIENT}"
            )
        }

        failure {
            echo 'Build failed. Sending email...'
            emailext(
                subject: "Build Failed: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
                body: "<p>Build <b>FAILED</b> in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>",
                to: "${RECIPIENT}"
            )
        }
    }
}
