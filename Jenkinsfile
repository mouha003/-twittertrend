pipeline {
    agent {
       node {
         label "jenkins-slave"
      }
    }
    environment{
		PATH="/usr/share/maven/bin:$PATH"
	}
    stages {
        stage('Build') {
            steps {
                echo '<--------------- Building --------------->'
                sh 'printenv'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '<------------- Build completed --------------->'
            }
        }

	stage('Unit Test'){
		steps {
			echo '<------ Unit Testing Started ----------->'
			sh 'mvn surefire-report:report'
			echo '<----- Unit Testing Stopped ------->'
		}
	}

	stage ("Sonar Analysis") {
            environment {
               scannerHome = tool 'my-sonarscanner'
            }
            steps {
                echo '<--------------- Sonar Analysis started  --------------->'
                withSonarQubeEnv('sonarqube-cloud') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }    
                echo '<--------------- Sonar Analysis stopped  --------------->'
            }   
        }    
   

   	stage("Quality Gate"){
		steps {
			script {
				echo '<------- Sonar Gate Analysis Started -----------'
				timeout(time: 1, unit: 'HOURS'){
					def qg = waitForQualityGate()
						if(qg.status != 'OK') { 
							error "Pipeline failed due to quality gate failure: ${qg.status}"
					}
			}
		   echo '<------ Sonar Gate Analysis Ends -------->'
		} 
	}
     }
  }
}
