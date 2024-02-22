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
    

}

}