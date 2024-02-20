pipeline{
    agent any
    tools{
        nodejs 'node16'
        jdk 'jdk17'
    }
    environment{
        sonar_home = tool 'sonar-scanner'
    }
    stages{
        stage("Clean Workspace"){
            steps{
                cleanWs()
            }
            
        }
        stage("Git checkout"){
            steps{
                 git branch: 'main', url: 'https://github.com/eswari625/2048-game.git'
            }
           
        }
        stage("Sonarqube Analysis"){
            steps{
                withSonarQubeEnv('sonar-server'){
                    sh ''' $sonar_home/bin/sonar-scanner -Dsonar.projectName=2048Game \
                    -Dsonar.projectKey=2048Game '''
                }
            }
        }
        stage("Quality Gate"){
            steps{
                script{
                    waitForQualityGate abortPipeline:false, credentialsId: 'sonar'
                }
            }
        }
        stage("Build and push image"){
            steps{
                withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                sh 'docker build -t game .'
                sh 'docker tag game eswari25/game:latest'
                sh 'docker push eswari25/game:latest'
                }
                
            }
        }

    }
}