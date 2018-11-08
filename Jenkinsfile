pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.222.139.227', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.220.42.212', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat "winscp -i tomcat-demo.pem **/target/*.war ec2-user@18.222.139.227:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i tomcat-demo.pem **/target/*.war ec2-user@18.220.42.212:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
    
}