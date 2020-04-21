pipeline {
    agent any

    parameters {
         string(name: 'tomcat_test', defaultValue: '13.127.231.62', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.232.197.181', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to test'){
                    steps {
                        sh "scp -i /tmp/7pmbatchmumbai.pem **/target/*.war ec2-user@${params.tomcat_test}:/tmp"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /tmp/7pmbatchmumbai.pem **/target/*.war ec2-user@${params.tomcat_prod}:/tmp"
                    }
                }
            }
        }
    }
}