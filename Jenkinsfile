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
                 bat 'mvn deploy' //org.apache.maven.plugins:maven-help-plugin:2.1.1:evaluate \-Dexpression=project.version 
           }
          post{
              success{
                  emailext body: 'Deploy do Framework realizado com sucesso.\n Nova versao do projeto: ${POM_VERSION}', 
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
                  
              }
              failure{
                  emailext body: 'Deploy do Framework nao foi realizado. \nAnalisar resultados: $BUILD_URL. \n Versao do projeto: $(mvn -q \
    -Dexec.executable=echo \
    -Dexec.args='${project.version}' \
    --non-recursive \
    exec:exec),                                                             
                  subject: 'Deploy Nexus - $BUILD_STATUS', 
                  to: 'henrique.galli@atomicsolutions.com.br'
              }
          } 
      }
    }  
}