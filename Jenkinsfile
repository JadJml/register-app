pipeline {
     agent any

     tools {
         jdk "java17"
         maven "mven3"
     }

     stages {

         stage("Cleanup Workspace  ") {
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

         stage(" Code Retour "){
         steps {
                echo 'PeipLine : OK'
         }
         }

     }
 }