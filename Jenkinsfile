pipeline {
    agent any

    stages {
    stage('Clone-Repo') {
     	steps {
                git branch : 'main' ,
                url: 'https://github.com/prasad-srtech/trainosys.git'
	    	}
        }
	stage('Build') {
		steps {
            dir('/root/.jenkins/workspace/trainosys-script') {
                sh 'mvn install'
            }
			
		}
	}	
 
	stage('Deployment') {
	   steps {
		sh 'scp /root/.jenkins/workspace/trainosys-script/target/trainosys.war root@52.23.231.36:/root/tomcat/apache-tomcat-10.1.46/webapps'
	}
    }
}
}