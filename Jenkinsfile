@Library('dockerBuild') _

pipeline {
    agent any

    environment {
    
        CERT_PASSWORD = "Mrudul@123"
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
      
        
     stage('Build') {
           steps {
              script {
                    dockerBuild("${env.IMAGE_NAME}", "--build-arg CERT_PASSWORD=${env.CERT_PASSWORD}")
                }
            }
        }
        stage('Push') {
            steps {
              script {
                dockerPush("${env.USER}", "${env.PASSWORD}", "${env.IMAGE_NAME}", "${env.BUILD_ID}")
                }
            }
        }
        stage('Cleanup') {
            steps {
                sh "docker rmi $IMAGE_NAME:$BUILD_ID"
            }
        }
       
        stage('Create Docker Registry Secret') {
            steps {
                 withCredentials([
                    string(credentialsId: 'kubeconfig', variable: 'KUBE_CONFIG')
                ]) {
                    script {

                        def secretName = "${APP_NAME}"

                // Check if the secret already exists
                def secretCheck = sh(script: "kubectl get secret ${secretName} --ignore-not-found", returnStdout: true).trim()
                echo "Secret check output: '${secretCheck}'"

                def secretExists = secretCheck ? true : false
                echo "secretExists: ${secretExists}"

                if (!secretExists) {
                     // If the secret does not exist, create it
                     sh """
                     kubectl create secret docker-registry ${secretName} \
                     --docker-server=https://index.docker.io/v1/ \
                     --docker-username=${USER} \
                     --docker-password=${PASSWORD} \
                     --docker-email=${DOCKER_HUB_EMAIL} || true
                     """
                     echo "Secret ${secretName} created."
                 } else {
                     echo "Secret ${secretName} already exists. Skipping creation."
                   }
                 }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                 withCredentials([
                    string(credentialsId: 'kubeconfig', variable: 'KUBE_CONFIG')
                ]) {
                    script {
                        kubeDeploy ("${env.IMAGE_NAME}", "${env.APP_NAME}")
                    }
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                withCredentials([
                    string(credentialsId: 'kubeconfig', variable: 'KUBE_CONFIG')
                ])  {
                        script {
                         kubeVerify ("${env.APP_NAME}")
                    }
                }
            }
        }
    }
}
