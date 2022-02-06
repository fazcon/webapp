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
            sh 'id'
            sh 'docker run gesellix/trufflehog --json https://github.com/fazcon/webapp.git>trufflehog'
            sh 'cat trufflehog'
        }

    }

    stage('Source Composition Analysis'){
        steps{
            sh 'wget https://raw.githubusercontent.com/fazcon/webapp/master/owasp-dependency-check.sh'
            sh 'chmod +x owasp-dependency-check.sh'
            sh 'bash owasp-dependency-check.sh'

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
          sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@54.144.209.141:/opt/tomcat9/webapps/webapp.war'
        }
    }
  }
 }
}
