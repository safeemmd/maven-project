pipeline {
    agent any

    parameters {
         string(name: 'tomcat_test', defaultValue: '52.66.249.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.126.109.212', description: 'Production Server')
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
                        sh "scp -i /home/ec2-user/7pmbatchmumbai.pem a.txt ec2-user@${params.tomcat_test}:/tmp"
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