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

        stage('Deploy to Production'){
        steps{
            echo "deploy to production"
            timeout(time:5, unit:"DAYS"){
                input message:'Approve PRODUCTION Deployment ?'
            }
            build job: 'deploy-to-prod'
        }

        post {
            success {
                echo "code deployed to producion"
            }

            failure{
                echo "Deployment Failed"
            }
        }

        
    }
}
