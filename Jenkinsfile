pipeline {
    agent any

     stages {

        stage('Get Code') {
           steps {
              git 'https://github.com/mnieto1973/helloworld-master'
              
           }
        }
         stage('Build') {
           steps {
              echo ' Prueba de stage'
              echo workspace
              bat'whoami'
              bat 'hostname' 
           }
           
        }
        stage('Unit'){
            parallel{
                 stage('Unit') {
               steps {
                   catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                       echo workspace
                         bat '''
                           whoami
                           hotname
                           set PYTHONPATH=%WORKSPACE%
                           pytest --junitxml=result.unit.xml test/unit
                         
                          '''
                   }
               
               }
            }
                stage('Service') {
               steps {
                   catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                        echo workspace
                         bat '''
                          whoami
                           hotname
                         set FLASK_APP=app\\api.py
                         start flask run
                         start java -jar C:\\desarrollo\\librerias\\wiremock\\wiremock-standalone-3.5.4.jar --verbose --port 9090 --root-dir test\\wiremock
                        set PYTHONPATH=%WORKSPACE%
                        pytest --junitxml=result.rest.xml test/rest
                        
                  '''
                   }
               
               }
            }
            }
        }
        
            
             stage('Junit') {
               steps {
                 junit 'result*.xml'
                 echo 'Finalizado!!!'
               }
            }
     }
}
