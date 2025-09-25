pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/bharathd87/legend.git'
        BRANCH = 'main'
    }
    
    stages {
        stage('Clean Workspace') {
            steps {
                echo 'CLeaning workspace'
                deleteDir()
            }
        }
        stage('Lint') {
            steps {
                echo "Cloning the repo from Gitlab ........."
                git branch: "${BRANCH}",
                    url: "${GIT_REPO}",
                    credentialsId: 'git_bharath'
            }
        }
        stage('Build') {
            steps {
                sh 'dos2unix build.sh'
                sh 'chmod +x build.sh'
                sh 'bash build.sh'
            }
        }
    }
}
post {
    always {
        echo 'Pipeline finished'
    }
    unstable {
        echo 'Build marked as UNSTABLE!'
        emailext (
            to: 'bharath.hyr96@gmail.com',
            subject: "Build Unstable: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
            body: """<p>Build became <b>UNSTABLE</b> in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>"""
        )
    }
    failure {
        echo 'Build failed!'
        emailext (
            to: 'bharath.hyr96@gmail.com',
            subject: "Build Failed: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
            body: """<p>Build failed in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>"""
        )
    }
    success {
        echo 'Build succeeded!'
        emailext (
            to: 'bharath.hyr96@gmail.com',
            subject: "Build Success: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
            body: """<p>Build succeeded in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>
                     <p>See details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>"""
        )
    }
}

       
