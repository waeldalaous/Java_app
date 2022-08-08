pipeline{
    agent any
    tools{
        maven "mvn"
    }
    environment{
        dockerRegistry = "waeldalaous/Java_app"
        registryCred = 'dockerhub'
    }

    stages{
        stage('BUILD'){
            steps {
                catchError(message: 'catch failure') { 
                    script {
                        sh 'mvn clean install -DskipTests'
                    }
                }
            }
        }
        post{
            steps{
                success {
                echo 'Now archiving...'
                archiveArtifacts artifacts: "**/target/*.war"
                }
            }

        }
        

    }
}