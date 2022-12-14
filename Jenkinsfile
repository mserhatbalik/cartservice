    pipeline {
        agent any
        environment {
            imageName = "cartservice"
            registryCredentials = "nexus"
            dockerImage = ''
            }
        stages {
            stage('Code checkout') {
                steps {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'prodevopsprojects', url: 'git@github.com:prodevopsprojects/cartservice.git']]])
                }
            }
            stage('Building image') {
                steps{
                    script {
                    dockerImage = docker.build(imageName, "./src")
                    }
                }
            }
            stage('Uploading to Nexus') {
                steps{  
                    script {
                        docker.withRegistry( 'http://'+registry, registryCredentials ) {
                        dockerImage.push('latest')
                        }
                    }
                }
            }
            stage('Confirming build success') {
                steps{  
                    writeFile file: 'cartservice.txt', text: 'Build is completed successfully.'
                    }
            }
        }
    }
