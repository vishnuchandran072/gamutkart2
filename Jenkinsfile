pipeline {
    agent any

//	tools {
//		jdk 'jdk11'
//	}
        environment {
            JAVA_HOME = '/home/ubuntu/jdk-11.0.26+4-linux-x64'
            PATH = "${JAVA_HOME}/bin:${env.PATH}"
        }

//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    	steps {
	        	checkout scm
	    	}
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
		
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "123" scp target/gamutkart.war test@172.17.0.2:/home/test/apache-tomcat-9.0.102/webapps'
                sh 'sshpass -p "123" ssh test@172.17.0.2 "/home/test/apache-tomcat-9.0.102/bin/startup.sh"'
            }
        }
    }
}
