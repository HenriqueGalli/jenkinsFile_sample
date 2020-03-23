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
                    echo 'sucesso'
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