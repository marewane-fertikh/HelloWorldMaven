pipeline {
    agent any

    tools {
        maven 'Maven3'     // Nom tel que défini dans Jenkins > Tools
        jdk 'Java17'       // Nom tel que défini dans Jenkins > Tools
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -Dmaven.compiler.source=17 -Dmaven.compiler.target=17'
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Tag') {
            steps {
                script {
                    sh "git config user.email 'jenkins@local'"
                    sh "git config user.name 'Jenkins CI'"

                    def tagName = "build-${env.BUILD_NUMBER}"
                    sh "git tag ${tagName}"

                    withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/marewane-fertikh/HelloWorldMaven.git --tags"
                    }
                }
            }
        }
    }
}
