pipeline{
    agent any
    tools{
        nodejs 'node16'
        jdk 'jdk17'
    }
    environment{
        sonar_home=tool sonar-scanner
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
                withSonarqubeEnv('sonar-server'){
                    sh ''' $sonar_home/bin/sonar-scanner -Dsonar.projectName=2048Game \
                    -DsonarprojectKey=2048Game '''
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
                sh 'docker build -t 2048Game .'
                sh 'docker tag 2048Game 2048Game:latest'
                sh 'docker push 2048Game:latest'
            }
        }

    }
}