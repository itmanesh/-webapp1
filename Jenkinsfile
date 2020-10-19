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
    
        stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean package'
       }
    }
    stage ('Deploy-To-Tomcat') {
      steps {
        sh 'cp /var/lib/jenkins/workspace/test1/target/WebApp.war /home/ubuntu/Downloads/apache-tomcat-8.5.59/webapps/webapp.war'
       }      
     }       
  }
}
