pipeline {
 agent any
 tools {
   maven 'Maven'
}
 stages {
  stage ('Initialize') {
    steps {
      sh '''
               echo "PATH = ${PATH}"
               echo "M2_HOME = ${M2_HOME}"
      '''
    }
  }
   
  stages ('Check-for-secret'){
   steps{
   sh 'rm trufflehog || true'
   sh 'docker run gesellix/trufflehog --json https://github.com/sylngwu/webapp.git > trufflehog'
   sh ' cat trufflehog'
   } 
  }
  
  stage ('Build') {
     steps {
     sh 'mvn clean package'
     }
  }
  
  stage ('Deploy-To-Tomcat') {
   steps {
    sshagent(['tomcat']){
    sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.92.70.116:/home/ubuntu/tomcat/apache-tomcat-9.0.65/webapps/WebApp.war'
    }
   }
  
  }
}
}
