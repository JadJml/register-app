pipeline {
    agent any

    tools {
        jdk "java17"
        maven "maven3"
    }

    environment {
        APP_NAME = "register-app-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "jadweb"
        DOCKER_PASS = credentials("token-msr")   // ID des credentials Docker dans Jenkins
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }

    stages {

        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: "main",
                    credentialsId: "github",
                    url: "https://github.com/JadJml/register-app.git"
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }

        stage("Build & Push Docker Image") {
            steps {
                script {

                    // Build de l'image Docker
                    docker_image = docker.build("${IMAGE_NAME}")

                    // Connexion au registre Docker Hub et push
                    docker.withRegistry('', "token-msr") {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }

        stage("Code Retour") {
            steps {
                echo 'Pipeline : OK'
            }
        }
    }

    post {
        always {
            echo "Build terminé."
        }
        success {
            echo "Build réussi."
        }
        failure {
            echo "Build échoué."
        }
    }
}