pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                git branch: 'feature_fix_racecond', url: 'https://github.com/AlbertoCrisostomo/helloworld.git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Eyyyy, esto es Python. No hay que compilar!!!'
                echo WORKSPACE
                bat 'tree'
            }
        }

        stage('Start Servers') {
            parallel {
                stage('Flask Server') {
                    steps {
                        bat '''
                            set FLASK_APP=app\\api.py
                            set PATH=C:\\Python312;C:\\Python312\\Scripts;C:\\Program Files\\Java\\jdk-11;C:\\Program Files\\Java\\jdk-11\\bin;
                    
                            start flask run
                        '''
                    }
                }
                stage('WireMock Server') {
                    steps {
                        bat '''
                            start java -jar C:\\Users\\soporte_bcrp\\Documents\\unir_tarea\\wiremock-standalone-3.5.3.jar --port 9090 --root-dir test\\wiremock
                        '''
                    }
                }
            }
        }

        stage('Test') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat '''
                        set PATH=C:\\Python312;C:\\Python312\\Scripts;
                        set PYTHONPATH=%WORKSPACE%
                        pytest --junitxml=result-unit.xml test\\unit
                        pytest --junitxml=result-rest.xml test\\rest
                    '''
                }
            }
        }

        stage('Result') {
            steps {
                junit 'result*.xml'
                echo 'Finalizado con éxito!!!'
            }
        }
    }
}
