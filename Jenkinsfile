pipeline {
  agent any
  tools {
    maven 'MavenJen'
  }
  stages {
    stage ('Initialize'){
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
           '''
      }
    }
    stage('Check-Git-Secrets'){
        steps{
            sh 'rm trufflehog||true'
            sh 'docker run gesellix/trufflehog --json https://github.com/fazcon/webapp.git>trufflehog'
            sh 'cat trufflehog'
        }

    }
    stage ('Build') {
      steps {
        sh 'mvn clean package' 
      }
    }
    stage ('Deploy-to-Tomcat'){
      steps {
          sshagent(['devsecops']){
          sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@44.202.34.94:/opt/tomcat9/webapps/webapp.war'
        }
    }
  }
 }
}
