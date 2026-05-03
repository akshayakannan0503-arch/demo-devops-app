pipeline {
   agent any
   environment {
       IMAGE_NAME = "vigno1995/myapp"
   }
   stages {
       stage('Checkout') {
           steps {
               git branch: 'main', url: 'https://github.com/akshayakannan0503-arch/demo-devops-app.git'
           }
       }
       stage('Test') {
           steps {
               sh 'python3 test.py'
           }
       }
       stage('Run App') {
           steps {
               sh 'python3 app.py'
           }
       }
       stage('Docker Build') {
           steps {
               sh 'docker build -t $IMAGE_NAME .'
           }
       }
       stage('Docker Push') {
           steps {
               withCredentials([usernamePassword(
                   credentialsId: 'dockerhub',
                   usernameVariable: 'USER',
                   passwordVariable: 'PASS'
               )]) {
                   sh '''
                   echo $PASS | docker login -u $USER --password-stdin
                   docker push $IMAGE_NAME
                   '''
               }
           }
       }
       stage('Deploy') {
           steps {
               sh 'docker run -d -p 80:80 $IMAGE_NAME'
           }
       }
   }
}
