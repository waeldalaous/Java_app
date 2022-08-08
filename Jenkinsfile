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
                sh 'mvn clean install -DskipTests'
            }
        }
        post{
            success {
                echo 'Now archiving...'
                archiveArtifacts artifacts: "**/target/*.war"
            }

        }
        

    }
}