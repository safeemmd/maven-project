pipeline{
    agent any
    stages{
        stage('Init'){
            steps{
                echo "Testing...."
            }
        }

        stage('Build'){
            steps{
                echo "Building...."
                sh 'mvn clean package'
            }
            post{
                success{
                    echo "now archiving..."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to staging'){
            steps{
                echo "code delyed....."
                build job: 'deploy-to-stage'
            }
        }
    }
}