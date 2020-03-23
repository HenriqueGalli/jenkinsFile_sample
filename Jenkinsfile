def TAG_SELECTOR = "UNINTIALIZED"
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
              //bat "mvn -N help:effective-pom -Doutput"
             bat "mvn --batch-mode -U deploy"
             script{
                 TAG_SELECTOR = readMavenPom().getVersion()
                 //projectVersion = pom.getVersion()  
                }        
            } 
            post{
                success{
                    echo 'Get Version Success'
                    echo "${TAG_SELECTOR}"
                }
                failure{
                    echo 'falha'
                    echo "${TAG_SELECTOR}"
                }
            }        
        }  
      stage('Deploy'){
          steps{          //tentar capturar a versao da pom e exibir em POM_VERSION
                 bat 'mvn deploy' 
                 echo "${TAG_SELECTOR}"
           }
          post{
              success{
                  emailext <p>Deploy do Framework realizado com sucesso. \nVersão do Projeto: ${DEF, DEF=TAG_SELECTOR}</p>,                       
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework nao foi realizado. \nVersão do Projeto: $TAG_SELECTOR \nAnalisar resultados: $BUILD_URL.\n'   ,                                                           
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}