pipeline {
    agent any
    
    
    environment{
        
        APP ="app"
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
              // search for app 's' substitue 
                   //${APP}.*: This is the pattern to search for. It matches the content of the APP variable followed by any characters
                   //${APP}:${TAG}: This is the replacement pattern. It replaces the matched pattern with the content of the APP variable, a colon (:), and the content of the TAG variable.
                  // sed: Stream editor command.
                  // -i: In-place editing. This option tells sed to edit the file in place, meaning it will modify the file directly rather than producing output to the console.
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
