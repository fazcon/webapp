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
    stage ('Build') {
      steps {
        sh 'mvn clean package' 
      }
    }
    stage ('Deploy-to-Tomcat'){
        steps {
            sshagent(['tomcat'])
            sh 'scp -o StrictHostKeyChecking=no target/*.war' tomcat@44.202.34.94:/opt/tomcat9/webapps/
        }
    }
  }
}
