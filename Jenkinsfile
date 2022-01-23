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
    
   stage("Check-git-secrets"){
        steps{
            sh 'rm trufflehog || true'
            sh 'docker run gesellix/trufflehog --json https://github.com/bml13052000/webapp.git > trufflehog'
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
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@44.202.71.180:/opt/tomcat/latest/webapps/webapp.war'
              }      
           }       
    }
    
  }
}
