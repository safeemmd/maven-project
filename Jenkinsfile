pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '13.232.242.252', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.235.83.196', description: 'Production Server')
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
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /d/AWS/7pmbatchmumbai.pem **/target/*.war ec2-user@${params.tomcat_dev}:/home/ec2-user/apache-tomcat-8.5.53/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /d/AWS/7pmbatchmumbai.pem **/target/*.war ec2-user@${params.tomcat_prod}:/home/ec2-user/apache-tomcat-8.5.53/webapps"
                    }
                }
            }
        }
    }
}
