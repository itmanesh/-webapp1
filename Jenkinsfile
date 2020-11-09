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
    

    
        stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
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
    
    stage ('DAST') {
      steps {
        sshagent(['ssh']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@192.168.6.155 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://127.0.0.1:8081/webapp/" || true'
        }
      }
    }
  }
}
