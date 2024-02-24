pipeline{
    agent any
    tools{   // the tools that we will be using 
        jdk 'java17'  
        maven 'Maven3'

    }
    environment{
        APP_NAME="mycompletepipeline"
        RELEASE ="1.0.0"
        DOCKER_USER= "wasraz"  //here it is only used to create the image name and not for the login to the dockerhub registry
        DOCKER_PASS= "jenkins_dockerhub_access_token"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG="${RELEASE}-${BUILD_NUMBER}"

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

        stage (" Testing the Application"){
            steps{
                sh "mvn test"
            }
        } 

        stage (" SonarQube Code Analysis"){
            steps{
                script{
                    withSonarQubeEnv(installationName: 'SonarQube-Scanner' , credentialsId: 'jenkins_sonarqube_access_token'){
                        sh 'mvn sonar:sonar'
                    }

                }
                
            }
        } 

        stage (" Quality GATE"){
            steps{
                waitForQualityGate abortPipeline: false , credentialsId: 'jenkins_sonarqube_access_token' // abortPipeline: true ==> in order to abort the pipeline if the code analysis didn't succed
            }
        } 
        
        stage (" Docker Build and Push"){
            steps{
                script{
                    docker.withRegistry('', DOCKER_PASS){
                    docker_image = docker.build "${IMAGE_NAME}"
                }

                    docker.withRegistry('', DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                }

                }
                
                
            }
        } 

    

}

}