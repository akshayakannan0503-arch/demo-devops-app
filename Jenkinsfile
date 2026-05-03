pipeline {
   agent any
   tools {
       maven 'Maven'
   }
   stages {
       stage('Checkout') {
           steps {
               git 'https://github.com/akshayakannan0503-arch/demo-devops-app.git'
           }
       }
       stage('Build') {
           steps {
               sh 'echo "Build successful"'
           }
       }
       stage('SonarCloud Analysis') {
           steps {
               withCredentials([string(credentialsId: 'sonar-token', variable: 'TOKEN')]) {
                   sh '''
                   mvn sonar:sonar \
                   -Dsonar.projectKey=demo-devops-app \
                   -Dsonar.organization=akshayakannan0503-arch \
                   -Dsonar.host.url=https://sonarcloud.io \
                   -Dsonar.login=$TOKEN
                   '''
               }
           }
       }
       stage('Docker Build') {
           steps {
               sh 'docker build -t vigno1995/myapp .'
           }
       }
       stage('Docker Push') {
           steps {
               withCredentials([usernamePassword(
                   credentialsId: 'dockerhub',
                   usernameVariable: 'USER',
                   passwordVariable: 'PASS')]) {
                   sh '''
                   echo $PASS | docker login -u $USER --password-stdin
                   docker push vigno1995/myapp
                   '''
               }
           }
       }
       stage('Deploy') {
           steps {
               sh '''
               docker rm -f myapp || true
               docker run -d --name myapp -p 80:80 vigno1995/myapp
               '''
           }
       }
   }
}
