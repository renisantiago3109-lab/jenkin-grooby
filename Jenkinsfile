pipeline {
  agent any

  options {
    timestamps()
    ansiColor('xterm')
  }

  environment {
    // If you use a configured Maven installation in Jenkins:
    // MAVEN_HOME = tool name: 'M3', type: 'maven'
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
            WAR_FILE=$(ls target/*.war | head -n 1)
            echo "Deploying $WAR_FILE"
            scp -o StrictHostKeyChecking=no "$WAR_FILE" root@52.23.231.36:/root/tomcat/apache-tomcat-10.1.46/webapps/
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
