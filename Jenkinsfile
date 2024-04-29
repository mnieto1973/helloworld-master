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
              bat 'dir'
              
           }
           
        }
        stage('Tests'){
            parallel{
                 stage('Unit') {
               steps {
                   catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                         bat '''
                   set PYTHONPATH=%WORKSPACE%
                   pytest --junitxml=result.unit.xml test/unit
                  '''
                   }
               
               }
            }
                stage('Rest') {
               steps {
                   catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                         bat '''
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
        
            
             stage('Results') {
               steps {
                 junit 'result*.xml'
                 echo 'Finalizado!!!'
               }
            }
     }
}
