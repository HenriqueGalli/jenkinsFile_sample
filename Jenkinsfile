
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
            bat 'mvn clean compile package' 
            //bat' mvn help:evaluate -Dexpression=project.version -q -DforceStdout'           
         }        
         post{
             success {
              echo 'Clean and Compile succes...'        
              }
            }
        }
      stage('Get Version'){
              environment {
                PROJECT_VERSION = "UNINTIALIZED"  
              }
         steps{         
              bat "mvn -N help:effective-pom -Doutput"
              //bat "mvn --batch-mode -U deploy"
             script{
                  echo "Versão na stage Get Version antes do script ${PROJECT_VERSION}"     
                  PROJECT_VERSION = readMavenPom().getVersion()  
                  echo "Versão na stage Get Version depois do script ${PROJECT_VERSION}"             
                }                      
            } 
            post{
                always{
                        echo "Versão na stage Get Version ${PROJECT_VERSION}"   
                }
                
            }        
        }  
      stage('Deploy'){
          steps{          //tentar capturar a versao da pom e exibir em POM_VERSION
                 bat 'mvn deploy' 
                 //bat "setx TESTVERSION ${PROJECT_VERSION} /M"                       
           }
          post{
              success{   
                  emailext body: readFile("C:/Users/Atomic/Desktop/jenkinsFile_sample/Novo e-mail.html"),                       //Deploy do Framework realizado com sucesso. \nVersão do Projeto: 
                  subject: 'Deploy Nexus - $BUILD_STATUS ', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body:  readFile("C:/Users/Atomic/Desktop/jenkinsFile_sample/Novo e-mail.html")  ,                                                           
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}