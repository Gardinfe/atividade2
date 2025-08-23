pipeline {
    agent any

    // Variáveis de ambiente para o nome de usuário do Docker Hub
    // É uma boa prática não deixar nomes de usuário hardcoded
    environment {
        DOCKERHUB_USER = 'gardinfernando'
    }

    stages {
        stage('Cleanup') {
            steps {
                echo 'Limpando o workspace...'
                sh 'docker image prune -f || true'
                deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                echo 'Baixando o código do GitHub...'
                git url: 'https://github.com/Gardinfe/atividade2.git', branch: 'main'
            }
        }
        stage('Construção (Build)') {
            steps {
                echo 'Iniciando a construção das imagens Docker...'
                script {
                    echo 'Construindo imagem para o serviço WEB...'
                    docker.build("${DOCKERHUB_USER}/atividade2-web", './web')

                    echo 'Construindo imagem para o serviço DB...'
                    docker.build("${DOCKERHUB_USER}/atividade2-db", './db')
                }
            }
        }
        stage('Entrega (Delivery)') {
            steps {
                echo 'Enviando imagens para o Docker Hub...'
                // O 'docker-hub-credentials' deve ser o ID da sua credencial configurada no Jenkins
                withDockerRegistry(credentialsId: 'docker-hub-credentials') {
                    sh "docker push ${DOCKERHUB_USER}/atividade2-web"
                    sh "docker push ${DOCKERHUB_USER}/atividade2-db"
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline executada com sucesso!'
        }
        failure {
            echo 'A Pipeline falhou.'
        }
    }
}
        }
    }
}
