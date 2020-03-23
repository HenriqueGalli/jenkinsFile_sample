pipeline {
    
   agent any

   tools {
    maven 'localMaven'
    //pom = readMavenPom file: 'pom.xml'
    //pom.version
   }
    triggers{
        pollSCM('* * * * *')
    }
    stages{
      stage('Build') {
         steps {
            git url: 'https://github.com/HenriqueGalli/DeploySnap.git'   
           // projectVersion = pom.getVersion()  
            bat 'mvn clean compile package' 
            bat' mvn help:evaluate -Dexpression=project.version -q -DforceStdout'           
         }
         post{
             success {
              echo 'Clean and Compile succes...'
              //echo "${projectVersion}"
              }
            }
        }
      stage('Deploy'){
          steps{          //tentar capturar a versao da pom e exibir em POM_VERSION
                 bat 'mvn deploy' 
           }
          post{
              success{
                  emailext body: 'Deploy do Framework realizado com sucesso.\n' ,                            
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework nao foi realizado. \nAnalisar resultados: $BUILD_URL.\n'   ,                                                           
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}