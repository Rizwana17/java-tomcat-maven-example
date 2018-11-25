node{
   stage('SCM Checkout'){
     git 'https://github.com/rajnikhattarrsinha/java-tomcat-maven-example'
   }
   
   stage('Mvn Package'){
      // Get maven home path
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }
    stage ('Test'){
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
      sh "${mvnHome}/bin/mvn verify; sleep 3"
   
   }   
   stage('Build Docker Image'){
   sh 'docker build -t rajnikhattarrsinha/javatomcat:2.0.0 .'
   }
   
   
   stage('Publish Docker Image')
   {
      withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
    // some block
         sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
      sh 'docker push rajnikhattarrsinha/javatomcat:2.0.0'
   
   }
   
   stage('Pull Docker Image and Deploy'){
      def dockerRun= 'sudo docker run -p 8080:8080 -d --name java-tomcat-maven-$BUILD_NUMBER rajnikhattarrsinha/javatomcat:2.0.0'
      //def portChk='./portrel.sh'
      sshagent(['dockerdeployserver2']) {
    // some block
         sh "ssh -o StrictHostKeyChecking=no ubuntu@34.239.128.128 ${dockerRun}"   
                   
      }
   }
   
   /*stage('Run Container on Deployment-server'){
      def dockerRun= 'docker run -p 8080:8080 -d --name my-app rajnikhattarrsinha/my-app:2.0.0'
      sshagent(['deployserver']) {
    // some block
         sh "ssh -o StrictHostKeyChecking=no root@54.144.60.78 ${dockerRun}"
      }
   }
   */
   
   
}


