 pipeline{
    agent any
    
    tools {
        maven 'Maven_Home'  // pass same name which is define in jenkins global configuration
    }

    stages{
        stage("Test"){
            steps{
                //mvn test
                //sh "mvn --version"
                sh "mvn test"
                 //slackSend channel: 'rk', message: 'Job Started'
            }
           
        }

        stage("Build"){
            steps{
                //mvn install or package
                sh "mvn package"
            }
        }
		
        stage("Build Docker image"){
                  steps{
                      sh "docker build -t p9rahul/spring-boot-mongo ."
                  }
           }
	    
        stage("Docker Push"){
            steps{
			withCredentials([string(credentialsId: 'Docker_Hub_Credentials', variable: 'Docker_Hub_Credentials')]) {
			sh "docker login -u p9rahul -p ${Docker_Hub_Credentials}"
			}
				
                sh "docker push p9rahul/spring-boot-mongo "
            }
        }
	    
	  stage("Deploy on K8S cluster"){
            steps{
                KubernetesDeploy(
					configs: 'springBootMongo.yml',
					kubeconfigId: 'KUBERNETES_CLUSTER_CONFIG', //name cpoy from jenkins credential
					enableConfigSubstitution: true //use this when we use dynamic environment variable in manefest file
				)
            }
        }
       
    }
    
}
