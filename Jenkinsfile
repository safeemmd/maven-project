pipeline {
    agent any

    parameters {
         string(name: 'tomcat_test', defaultValue: '13.233.46.254', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.66.101.150', description: 'Production Server')
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
                        sh "scp -i /home/ec2-user/.ssh/id_rsa **/target/*.war ec2-user@${params.tomcat_test}:/home/ec2-user/apache-tomcat-8.5.53/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ec2-user/.ssh/id_rsa **/target/*.war ec2-user@${params.tomcat_prod}:/home/ec2-user/apache-tomcat-8.5.53/webapps/"
                    }
                }
            }
        }
    }
}