def PROJECT_VERSION = "UNINTIALIZED"     
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
          steps{
              bat "mvn -N help:effective-pom -Doutput"
              //bat "mvn --batch-mode -U deploy"
             script{
                  PROJECT_VERSION = readMavenPom().getVersion()             
                }   
               bat "set TEST_VERSION ${PROJECT_VERSION}"     
            } 
            echo $TEST_VERSION
             post{
                success{
                    echo 'Get Version Success'
                    echo "${PROJECT_VERSION}"
                }
                failure{
                    echo 'falha'
                    echo "${PROJECT_VERSION}"
                }
            }        
        }  
      stage('Deploy'){
          steps{          //tentar capturar a versao da pom e exibir em POM_VERSION
                 bat 'mvn deploy'                
           }
          post{
              success{                
                 echo "${TEST_VERSION}"
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