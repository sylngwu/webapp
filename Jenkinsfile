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
                sh 'scp -o StrictHostKeyChecking=yes target/*.war ubuntu@54.159.150.52:/home/ubuntu/prod/apache-tomcat-9.0.85/webapps/webapp.war'
              }      
           }       
    }
  }
}
