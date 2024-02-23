pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main'], [name: '*/dev']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-ssh', url: 'https://github.com/abrkristin/cicd-pipeline']])
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh '/var/lib/jenkins/workspace/CD_deploy_manual/scripts/test.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = ""

                    if (env.BRANCH_NAME == 'main') {
                        imageName = "abrkristin/nodemain:v1.0"
                    } else if (env.BRANCH_NAME == 'dev') {
                        imageName = "abrkristin/nodedev:v1.0"
                    } else {
                        error "No matching branch for Docker image"
                    }
                    
                    sh "docker build -t ${imageName} ."
                }
            }
        }
    }


    post {
        success {
            echo 'Pipeline completed successfully. Perform any post-build steps here.'
        }

        failure {
            echo 'Pipeline failed. Perform any post-failure steps here.'
        }
    }
}
