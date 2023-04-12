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
	stage ("Sonar Analysis") {
            environment {
               scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                echo '<--------------- Sonar Analysis started  --------------->'
                withSonarQubeEnv('SonarServer') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }    
                echo '<--------------- Sonar Analysis stopped  --------------->'
            }   
        }    

}
