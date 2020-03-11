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
          steps{          //tentar capturar a versao da pom e exibir em POM_VERSION
                 bat 'mvn org.apache.maven.plugins:maven-help-plugin:2.1.1:evaluate -Dexpression=project.version deploy' 
           }
          post{
              success{
                  emailext body: 'Deploy do Framework realizado com sucesso.', 
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework nao foi realizado. \nAnalisar resultados: $BUILD_URL.\nVersao: $POM_VERSION.',                                                            
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}