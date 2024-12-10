pipeline {
    environment {
        IMAGEN = "ezequiellemos/django_tutorial" // Cambia esto por el nombre de tu imagen en Docker Hub.
        LOGIN = 'USER_DOCKERHUB' // Debe coincidir con el ID de las credenciales en Jenkins.
    }
    agent any
    stages {
        stage("Preparar_entorno") {
            agent {
                docker {
                    image "python:3" // Usar imagen de Python.
                    args '-u root:root'
                }
            }
            stages {
                stage('Clonar_repositorio') {
                    steps {
                        git branch: 'master', url: 'https://github.com/EzequielLemos/django_tutorial'
                    }
                }
                stage('Instalar_dependencias') {
                    steps {
                        sh 'pip install -r django_tutorial/requirements.txt'
                    }
                }
                stage('Ejecutar_pruebas') {
                    steps {
                        sh 'cd django_tutorial && python manage.py test --settings=django_tutorial.desarollo'
                    }
                }
            }
        }
        stage("Construir_y_subir_imagen") {
            stages {
                stage('Construir_imagen') {
                    steps {
                        script {
                            newApp = docker.build "$IMAGEN:latest"
                        }
                    }
                }
                stage('Subir_imagen') {
                    steps {
                        script {
                            docker.withRegistry('', 'USER_DOCKERHUB') {
                                newApp.push()
                            }
                        }
                    }
                }
                stage('Borrar_imagen_local') {
                    steps {
                        sh "docker rmi $IMAGEN:latest"
                    }
                }
            }
        }
    }
}
