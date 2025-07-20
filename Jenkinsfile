pipeline {
   agent any

   environment {
      IMAGE_NAME = "java-app"
      IMAGE_TAG ="latest"
      }
   stages {
      stage('Clone Code') {
         steps {
            git url : 'https://github.com/ThanushKumarVusa/Java-app.git'
            }
         }
   stage('Build with Maven') {
      steps {
         sh 'mvn clean package -DskipTests'
         }
        }
   stage('Build Docker Image') {
      steps {
         sh '''
            docker build -t $IMAGE_NAME:$IMAGE_TAG .
            '''
            }
            }
   stage('Run Docker Container') {
      steps {
         sh '''
            docker rm -f java-app || true
            docker run -d --name java-app -p 8080:8080 $IMAGE_NAME:$IMAGE_TAG
            '''
            }
            }
           }
   post{
      success{
         echo 'YES'
         }
      failure {
         echo 'You Failed'
         }
         }
         }

