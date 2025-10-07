pipeline {
  agent any

  tools {
     maven 'maven.3.9.11'  
  }
  stages {
    stage('Checkout') {
      steps {
        // If the repo is public, this is fine. If private, add credentialsId: 'git-creds'
        git branch: 'main', url: 'https://github.com/prasad-srtech/trainosys.git'
      }
    }

    stage('Build') {
      steps {
        // Build in the workspace; no need for hard-coded paths
        sh 'mvn -B -DskipTests clean package'
        // If you actually need tests: sh 'mvn -B clean package'
      }
    }

    stage('Deploy') {
      steps {
        // Use an SSH credential stored in Jenkins (Manage Jenkins > Credentials)
        sshagent(credentials: ['server-ssh']) {
          // Disable host key prompt the first time; or pre-add the host key
          sh '''
            docker cp /var/jenkins_home/workspace/trainosys-clone/target/trainosys.war f74fb08808dc:/usr/local/tomcat/webapps/trainosys.war  
          '''
        }
      }
    }
  }

  post {
    success {
      archiveArtifacts artifacts: 'target/*.war', fingerprint: true
    }
    always {
      junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
    }
  }
}
