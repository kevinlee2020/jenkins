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
		registry = "klee2020/jenkins"
		registryCredential = 'klee2020-docker'
                dockerImage = ''
		k8s_config = credentials('jenkins-k8s-config')
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

      stage('compile') {
        steps {
	      	sh 'mvn compile'
              }
          }

      stage('test') {
        steps {
		sh 'mvn test'
              }
          }

      stage('package') {
        steps {
		sh 'mvn package'
              }
          }

      stage('Build docker master') {
        steps {
		script{
                         def dockerImage= docker.build registry + ":$BUILD_NUMBER"
                         docker.withRegistry( '', registryCredential ) {
                         dockerImage.push()
                              }
			 }
                  }
         }

      stage('waiting for image push to docker hub') {
        steps {
		sh 'sleep 180'
              }
          }

      stage('Doploy images') {
        steps {
          if (env.BRANCH_NAME == 'master') {
            input "If release it?"
            }  
		sh "echo ${k8s_config} > ~/config" 
	        sh 'sed -i "s/TAG/${BUILD_NUMBER}/g" helloworld.yml'
		sh 'kubectl apply -f ./helloworld.yml'
		sh 'kubectl apply -f ./helloworld-nodeport-service.yml'
			}	
		}
	}


}
