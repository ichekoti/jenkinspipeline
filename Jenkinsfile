pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: 'C://Tomcat_8.0/webapps', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'C://Tomcat_8.0-Production//webapps', description: 'Production Server')
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
                    archiveArtifacts artifacts: '**\target\*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "copy **\target\*.war C:\Tomcat_8.0\webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
						bat "copy **\target\*.war C:\Tomcat_8.0-Production\webapps"
                    }
                }
            }
        }
    }
}