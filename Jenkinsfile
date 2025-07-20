pipeline {
   agent any
   
   tools {
      jdk 'Java-17'  // This must match the JDK name configured in Jenkins
      maven 'Maven'   // Optional: specify Maven version if needed
   }
   
   environment {
      IMAGE_NAME = "java-app"
      IMAGE_TAG = "latest"
      JAVA_HOME = tool 'Java-17'  // Explicitly set JAVA_HOME
      PATH = "${JAVA_HOME}/bin:${env.PATH}"
   }
   
   stages {
      stage('Clone Code') {
         steps {
            git url: 'https://github.com/ThanushKumarVusa/Java-app.git'
         }
      }
      
      stage('Check Java Version') {
         steps {
            sh '''
               echo "Java version check:"
               java -version
               javac -version
               echo "JAVA_HOME: $JAVA_HOME"
               echo "Maven version:"
               mvn -version
            '''
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
   
   post {
      success {
         echo 'YES'
      }
      failure {
         echo 'You Failed'
      }
   }
}
