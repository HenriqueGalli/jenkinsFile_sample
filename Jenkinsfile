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
              echo 'Clean and Compile succes...'
              }
            }
        }
      stage('Deploy'){
          steps{
                 bat 'mvn org.apache.maven.plugins:maven-help-plugin:2.1.1:evaluate \
    -Dexpression=project.version deploy' 
           }
          post{
              success{
                  emailext body: 'Deploy do Framework realizado com sucesso.\n Nova versao do projeto: $PROJECT.VERSION', 
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework nao foi realizado. \nAnalisar resultados: $BUILD_URL. \n Versao do projeto: $PROJECT.VERSION', 
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}