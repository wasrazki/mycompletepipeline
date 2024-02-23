pipeline{
    agent any
    tools{   // the tools that we will be using 
        jdk 'java17'  
        maven 'Maven3'

    }

    stages{
        stage (" Cleanup Workspace"){
            steps{
                cleanWs()   // fonction prédefinie to clean the workspace

            }

        } 

        stage (" Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github_access_token', url: 'https://github.com/wasrazki/mycompletepipeline'
            }
        } 

         stage (" Building the Application"){
            steps{
                sh "mvn clean package"
            }
        } 

         stage (" Testin the Application"){
            steps{
                sh "mvn test"
            }
        } 

        stage (" SonarQube Code Analysis"){
            steps{
                withSonarQubeEnv(credentialsId: 'jenkins_sonarqube_access_token'){
                    sh 'mvn sonar:sonar'
                }
            }
        } 
    

}

}