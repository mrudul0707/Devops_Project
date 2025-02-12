@Library('dockerBuild') _

pipeline {
    agent any

    environment {
    
        CERT_PASSWORD = credentials('CERT_PASSWORD')
        IMAGE_NAME = "mrudul0707/dotnet-app"
        PASSWORD = "Mrudul@123"
        REGISTRY = "https://registry-1.docker.io/v2/"
        USER = "mrudul0707"
        APP_NAME = "dotnet-app"
        DOCKER_HUB_EMAIL = "mrudulnarkhede90@gmail.com"
}


    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/mrudul0707/Devops_Project.git'
            }
        }
         
        stage('Trivy FS Scan ') {
            steps {
               echo 'hello'
            }
        }

         
        stage('SonarQube Analysis ') {
            steps {
                echo 'hello'
            }
        }

        stage('Maven Build') {
           steps {
              script {
                  echo 'hello'
                }
            }
        }
        stage('Publish Artifacts ') {
            steps {
              script {
               echo 'hello'
                }
            }
        }
       
    
         stage('create docker registry secret') {
            steps {
                
                    script {
                        echo 'hello'
                    }
                
            }
        }

        stage('Docker Build & Tag') {
            steps {
                sh "trivy image --format table -o image.html mrudul0707/my_prototype:latest"
            }
        }
        stage('Trivy Image Scan ') {
            steps {
                echo 'hello'
            }
        }
       
        stage('Deploy to Minikube ') {
            steps {
                
                    script {
                        echo 'hello'
                    }
                
            }
        }
        stage('Verify Deployment') {
            steps {
                        script {
                            echo 'hello'
                    }
                
            }
        }
    }
}
