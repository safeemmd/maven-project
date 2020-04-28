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
                sh "mvn clean package"
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }
        
        stage('Deploy'){
            steps{
                echo "code delyed...."
            }
        }
    }
}