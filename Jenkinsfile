pipeline{
	agent any
	tools {
		maven 'maven'
	}
	stages{
		stage ('initialize'){
			steps{
				sh '''
						echo "PATH = ${PATH}"
						echo "M2_HOME = ${M2_HOME}"		
				''' 
            }
		}
		
		stage ('Check-Git-Secrets') {
			steps {
				sh 'rm trufflehog || true'
				sh 'docker run gesellix/trufflehog --json https://github.com/aw-ali/javaapp.git > trufflehog'
				sh 'cat trufflehog'
			}
		}
		
		
		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
		}
		stage ('Deploy-To-Tomcat') {
                         steps {
                                 sshagent(['tomcat']) {
                                 sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.197.44.168:apache-tomcat-8.5.58/webapps/webapp.war'
                 }      
              }       
    }
	
	}

}
