pipeline {
    agent {
       node {
         label "jenkins-slave"
      }
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
	stage('sonar analysis')
		environment {
			scannerHome = tool 'my-sonarscanner'
	}
	steps{
		echo '<--------------- Sonar Analysis --------------->'
		withSonarQubeEnv('sonarqube-cloud')
		sh "${scannerHome}/bin/sonar-scanner"
		echo '<--------------- Sonar Analysis --------------->'
	}
    }

}
