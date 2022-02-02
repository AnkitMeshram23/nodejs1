pipeline {

agent any

tools {nodejs "Nodejs"}

environment {

CI= 'true'

}
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',		    
		credentialsId: 'bb059738-bc0c-439c-af36-4c5aeeb1128e',
			url: 'https://github.com/AnkitMeshram23/nodejs1.git'    
	    }
	}	
        stage('Building') {
            steps {
                sh 'npm install'
            }
        }		    
	    
        stage('Sonar Analysis') {
environment {
SCANNER_HOME = tool 'Anksonar'
PROJECT_NAME = "node"
}
steps {
withSonarQubeEnv('Anksonar') {
sh '''$SCANNER_HOME/bin/sonar-scanner \
-Dsonar.java.binaries=build/classes/java/ \
-Dsonar.projectKey=$PROJECT_NAME \
-Dsonar.sources=.'''
}
}
}
         stage('Quality Gate') {
           steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline:true
              }
            }
          }
	    
	     stage('Testnode') {
            steps {
                sh 'npm test'
            }
        }
	    stage('Package') {
            steps {
                sh 'npm run build'
            }
        }
	stage('Deploy') {
            steps {
		    sh 'npm start'
            }
        }
  }
}
