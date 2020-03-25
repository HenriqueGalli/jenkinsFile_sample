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
                                 
            } 
            post{
                always{
                    script{
                        import hudson.model.*;
                        import hudson.util.*;
                        def thr = Thread.currentThread();
                        def currentBuild = thr?.executable;
                        def mavenVer = currentBuild.getParent().getModules().toArray()[0].getVersion();
                        def newParamAction = new hudson.model.ParametersAction(new hudson.model.StringParameterValue("MAVEN_VERSION", mavenVer));
                        currentBuild.addAction(newParamAction);
                    }
                }
            }        
        }  
      //stage('Edit XML'){
      //      steps{
      //          script{
      //              def file = new File 'exemploXMLVERSAO.xml'
      //          }
      //      }
      //  } 
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