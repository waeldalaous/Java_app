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

        stage('Code Analysis with SONARQUBE'){
            environment{
                scannerHome = tool 'sonarScanner'

            }
            steps{
                withSonarQubeEnv(credentialsId: 'sonar_token', installationName: 'sonnarscanner') {
                    echo "done"

                }
                /*withSonarQubeEnv(credentialsId: 'sonarqube-token') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=java_app \
                   -Dsonar.projectName=java_app \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }*/
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