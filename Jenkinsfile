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

        stage('Code Analysis with SONARQUBE') {
            environment{
                scannerHome = tool 'sonarScanner'

            }
            steps{
                
                withSonarQubeEnv(credentialsId: 'sonar_token',installationName: 'sonarqube-server') {
                    withCredentials([string(credentialsId: 'Sonar', variable: 'sonar')]) {
                        sh '''
                            mvn sonar:sonar \
                            -Dsonar.projectKey=java_app \
                            -Dsonar.host.url=http://192.168.198.136:9000 \
                            -Dsonar.login=$sonar
                            '''

                    } 
                    
                }

            }
            
        }

        

        stage('Image build & Image push'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]){
                        sh '''
                            docker login -u ${docker_user} -p ${docker_pass}
                            docker build -t waeldalaous/java_app:$BUILD_NUMBER .
                            docker push waeldalaous/java_app:$BUILD_NUMBER 
                            docker rmi waeldalaous/java_app:$BUILD_NUMBER 

                        '''
                    }
                }
            }
            post{
                success{
                    echo "image pushed to dockerhub"
                }
            }
        }


        
    }
}