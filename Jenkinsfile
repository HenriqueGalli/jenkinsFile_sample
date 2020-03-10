pipeline {
    
   agent any

   tools {
    maven 'localMaven'
   }
    triggers{
        pollSCM('* * * * *')
    }
    stages{
      stage('Build') {
         steps {
            // Get some code from a GitHub repository
            git url: 'https://github.com/HenriqueGalli/maven-project'
            bat 'mvn clean compile package' 
         }
         post{
             success {
              echo 'Build succes...'
              }
            }
        }
      stage('Deploy'){
          steps{
                 bat 'mvn deploy' 
           }
          post{
              success{
                  emailext body: 'Deploy do Framework realizado com sucesso.', 
                  subject: 'Deploy Nexus', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework não foi realizado.', 
                  subject: 'Deploy Nexus', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}