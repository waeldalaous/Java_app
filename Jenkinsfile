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
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: "**/target/*.war"
                }
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

        stage('CODE ANALYSIS with CHECKSTYLE'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
            post{
                success{
                    echo 'Generated analysis result'
                }
            }
        }

        stage('Image build'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]){
                        sh '''
                            docker login -u ${docker_user} -p ${docker_pass}

                        '''
                    }
                }
            }
        }


        
    }
}