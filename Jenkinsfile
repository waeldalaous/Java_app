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

        stage('UNIT test'){
            steps{
                sh 'mvn test'
            }
        }

        stage('INTEGRATION test'){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        
    }
}