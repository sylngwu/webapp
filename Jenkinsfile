pipeline {
  agent any 
  tools {
    maven 'maven'
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
stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/sylngwu/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/sylngwu/webapp/master/owasp-dependency-check.sh"'
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.161.201.197:/home/ubuntu/prod/apache-tomcat-9.0.85/webapps/webapp.war'
              }      
           }       
    }
  }
}
