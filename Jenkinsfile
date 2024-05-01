pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                git 'https://github.com/Alfonso-Caballero/Caso-Practico-1.1.git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Esto es python'
                echo WORKSPACE
                dir
            }
        }
        
        stage('Unit') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                    bat '''
                        set PYTHONPATH=.
                        pytest --junitxml=result-unit.xml test\\unit
                        '''
                }
            }
        }
        
        stage('Rest') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                    bat '''
                        set FLASK_APP=app\\api.py
                        start flask run
                        start java -jar C:\\Users\\alfon\\My_Projects\\Caso-Practico-1.1\\test\\wiremock\\wiremock-standalone-3.5.4.jar --port 9090 --root-dir test\\wiremock
                        set PYTHONPATH=.
                        pytest --junitxml=result-rest.xml test\\rest
                        '''
                    }
                }
            }
        
        stage('Results') {
            steps {
                junit 'result*.xml'
                echo 'FINISH!'
            }
        }
    }
}