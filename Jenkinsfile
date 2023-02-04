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
    
    stage ('Build') {
      steps {
       sh 'mvn clean package'
    }
    }
    
       stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=yes target/*.war cloud_user@34.242.160.54:/home/cloud_user/prod/apache-tomcat-10.0.27/webapps/webapp.war'
              }      
           }       
    }
  }
}
    
    
    
