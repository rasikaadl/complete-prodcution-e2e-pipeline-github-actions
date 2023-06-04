pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
	
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'stage', credentialsId: 'github-access-token', url: 'https://github.com/rasikaadl/project-1-application'
            }
	    }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
	    }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }
	    }

        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqube-access-token') {
                        sh "mvn sonar:sonar"
                    }
                }  
            } 
        } 

        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-access-token' 
                }
            }
        }
    }
}
