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
            git url: 'https://github.com/HenriqueGalli/DeploySnap.git'
            bat  'mvn BUILD_VERSION=$(mvn help:evaluate -Dexpression=project.version -q -DforceStdout)'
            bat 'mvn clean compile package' 
            echo  $BUILD_VERSION
         }
         post{
             success {

              echo 'Clean and Compile succes...'
              echo  $BUILD_VERSION
              }
            }
        }
      stage('Deploy'){
          steps{          //tentar capturar a versao da pom e exibir em POM_VERSION
                 bat 'mvn deploy' 
           }
          post{
              success{
                  emailext body: 'Deploy do Framework realizado com sucesso.\n', 
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework nao foi realizado. \nAnalisar resultados: $BUILD_URL.\n',                                                            
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}