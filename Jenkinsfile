pipeline {
    agent {
        node {
            label 'master'
             }
          }
    
    tools{
        maven 'Maven'
        jdk 'jdk8'
        }
		
	environment {
		registry = "kevinlee2020/jenkins"
		registryCredential = 'klee2020-docker'
                dockerImage = ''
		}

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout scm
                echo "current branch: $BRANCH_NAME"
            }
        }
                 
        stage('Build docker master') {
            when {
                branch 'master' 
               }
                 
            steps {
					script{
                         def dockerImage= docker.build registry + ":master"
                             docker.withRegistry( '', registryCredential ) {
                             dockerImage.push()
                              }
			 }
                  }
         }



    }   
}
