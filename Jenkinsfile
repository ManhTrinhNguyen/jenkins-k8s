

pipeline {
    agent any
    tools {
        gradle 'gradle-9.0'
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
                    sh "echo ${version}"
                }
            }
        }

        // stage('Docker Build and Push') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'docker_credential', usernameVariable: 'USER', passwordVariable: 'PWD')]) {
        //             sh '''
        //                 echo $PWD | docker login --username AWS --password-stdin $ECR_REPO
        //                 docker build -t $ECR_REPO/$IMAGE_NAME .
        //                 docker push $ECR_REPO/$IMAGE_NAME
        //             '''
        //         }
        //     }
        // }
    }
}