pipeline {
    agent any

    environment {
        rname = "front"
        rurl = 'manelgaraya'
        imagename = "front"
        dockerhubCredentials = 'dockerhub'  // Replace with your Docker Hub credentials ID
    }

    stages {
        stage('build') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }
     
        stage('test') {
            steps {
                script {
                    echo 'testing'
                }
            }
        }

        stage('build image') {
            steps {
                script {
                    def imageName = "${rurl}/${imagename}:latest"
                    
                    bat "docker build -t ${imageName} ."
                    
                    // Use Docker Hub credentials to log in
                    withCredentials([usernamePassword(credentialsId: dockerhubCredentials, passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        bat "docker login -u ${env.DOCKERHUB_USERNAME} -p ${env.DOCKERHUB_PASSWORD}"
                    }
                    
                    bat "docker push ${imageName}"
                }
            }
        }
      stage('cleanup') {
            steps {
                script {
                    def imageName = "${rurl}/${imagename}:latest"
                    
                 
                    
                    bat "docker image rmi ${imageName}"
                      bat "docker logout"
                }
            }
        }
    }
}
