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
        sh 'docker run gesellix/trufflehog --json https://github.com/itmanesh/-webapp1.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
        stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
    
    stage ('DAAST') {
      steps{([$class: 'BuildScanner', incScan: false, incScanId: '', profile: '11111111-1111-1111-1111-111111111111', repTemp: '11111111-1111-1111-1111-111111111111', stopScan: true, svRep: false, target: '29a9362b-87b3-4097-b565-f051c7b9fe49', threat: 'DoNotFail'])
           }}
  }
}
