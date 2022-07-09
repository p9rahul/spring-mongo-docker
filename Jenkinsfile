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
				
                sh "docker push -t p9rahul/spring-boot-mongo "
            }
        }
       
    }
    
}
