// Powered by Infostretch



pipeline {
    agent any
    tools {
        maven "maven 3.6"
    }
     environment {
        NEXUS_ARTIFACT_VERSION= "${env.BUILD_NUMBER}"
    }

    options {
        parallelsAlwaysFailFast()
    }
    stages {
        stage('Non-Parallel Stage') {
            steps {
                echo 'This stage will be executed first.'
            }
        }
        stage('Parallel Stage') {
            parallel {
                   stage('Checkstyle') {
                        steps{
                            // Run the maven build with checkstyle
                            sh "mvn clean package checkstyle:checkstyle"
                         }
                     }
                    stage('Sonarqube') {
                        steps {
                            withSonarQubeEnv('SonarQube') {
                            //sh "mvn  clean package sonar:sonar -Dsonar.host_url=$SONAR_HOST_URL "
                            echo "SonarQube"
                            }
                         }
                    }
            }
        }
        stage('Publish in Nexus') {
            steps {
                nexusPublisher nexusInstanceId: 'Nexus',
                nexusRepositoryId: 'releases',
                packages: [[$class: 'MavenPackage',
                mavenAssetList: [[classifier: '', extension: '', filePath: 'target/conference-app-3.0.0.war']], mavenCoordinate: [artifactId: 'conference-application', groupId: 'de.codecentric', packaging: 'war', version: NEXUS_ARTIFACT_VERSION]]]
            }
        }
        stage('Build image') {
            steps{
                script{
                    def customImage = docker.build("conference-app-project")
                }
            }
        }
        stage('Run Test image') {
            parallel {
                stage('UAT') {
                    steps {
                        sh 'docker ps -q --filter name="conference-app-uat" | xargs -r docker stop'
                        sh 'docker container prune -f'
                        sh 'docker run -d --name conference-app-uat -p 8290:8080 conference-app-project:latest'

                    }
                }

            }
        }



    }
}