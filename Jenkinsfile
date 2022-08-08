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
                sh 'mvn clean install -DskipTests > log-file.log'
            }
        }

        stage('UNIT test'){
            steps{
                sh 'mvn -X test'
            }
        }
        
    }
}