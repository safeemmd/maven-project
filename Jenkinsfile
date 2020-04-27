pipeline {
    agent any

    parameters {
         string(name: 'tomcat_test', defaultValue: '15.206.174.208', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '15.206.90.187', description: 'Production Server')
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

    // stage("Method 2 ssh") {
	// steps {
	// 	sshagent (credentials: ['creds-id']) {
	// 	sh '''
    //         ssh ec2-user@13.233.118.219 'hostname && ls -la'
  			
	// 	'''
	// 	}
	// }
    // }

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