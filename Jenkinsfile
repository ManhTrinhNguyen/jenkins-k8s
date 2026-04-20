

pipeline {
    agent any
    tools {
        gradle 'gradle-8.7'
    }
    environment {
        ECR_REPO = '660753258283.dkr.ecr.us-west-1.amazonaws.com'
    }

    stages {
        stage('Increment Version') {
            steps {
                script {
                    sh 'gradle patchVersionUpdate'
                    def version = readProperties(file: 'version.properties')
                    env.IMAGE_NAME = "java:${version.major}.${version.minor}.${version.patch}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'gradle clean build'
                    
                }
                
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'ecr_credential', usernameVariable: 'USER', passwordVariable: 'PWD')]) {
                        sh "echo $PWD | docker login --username ${USER} --password-stdin ${ECR_REPO}"
                        sh "docker build -t ${ECR_REPO}/${IMAGE_NAME} ."
                        sh "docker push ${ECR_REPO}/${IMAGE_NAME}"
                    }
                }
            }
        }
    }
}