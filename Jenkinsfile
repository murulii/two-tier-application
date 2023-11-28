pipeline {
    agent any
    
    
    environment{
        
        APP ="register-app-pipeline"
        RELEASE = "1.0"
        
        DOCKER = "murulii"
        IMG_NAME = "${DOCKER}" + "/" + "${APP}"
        TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/murulii/two-tier-application.git'
            }
        }
        
        stage('build a image') {
            steps {
                sh '''
                
                docker pull mysql
                docker build -t app .
                docker image list
                docker tag app "${IMG_NAME}:${TAG}"
                
                
                '''
            }
        }
        
        
        stage('docker push') {
            steps {
                sh '''
                
                docker login -u murulii -p Depl0y@123
                
                docker push "${IMG_NAME}:${TAG}"
                
                '''
            }
        }
        
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   
                   sed -i 's/${APP}.*/${APP}:${TAG}/g' deployment.yaml



                   cat deployment.yaml

                """
            }
        }
        
        
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "murulii"
                   git config --global user.email "muruli800@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/murulii/two-tier-application.git main"
                }
            }
        }
        
    }
}
