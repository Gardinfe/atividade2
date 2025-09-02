pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'gardinfernando'
    }

    stages {
        stage('Cleanup') {
            steps {
                echo 'Limpando o workspace...'
                sh 'docker image prune -af || true'
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
                    docker.build("${DOCKERHUB_USER}/atividade2-web:latest", "-f ./web/Dockerfile.web ./web")
                    docker.build("${DOCKERHUB_USER}/atividade2-db:latest", "-f ./db/Dockerfile.mysql ./db")
                }
            }
        }
        stage('Entrega (Delivery)') {
            steps {
                echo 'Enviando imagens para o Docker Hub...'
                withDockerRegistry(url: 'https://index.docker.io/v1/', credentialsId: 'Docker_hub') {
                    sh "docker push ${DOCKERHUB_USER}/atividade2-web:latest"
                    sh "docker push ${DOCKERHUB_USER}/atividade2-db:latest"
                }
            }
        }
        stage('Deploy (Implantação)') {
            steps {
                echo 'Iniciando o deploy no servidor de destino...'
                sshagent(credentials: ['servidor-ssh']) { 
                    sh '''
                        ssh -o StrictHostKeyChecking=no fernando@192.168.100.6 <<EOF

                        # Garante que o repositório com o docker-compose.yml exista
                        cd ~/Documents/GitHub/Projetos/atividade2
                        git pull

                        # Baixa as imagens mais recentes construídas pelo Jenkins
                        echo "Baixando novas versões das imagens..."
                        docker compose pull
                        
                        # Sobe os serviços com as novas imagens em modo detached (-d)
                        echo "Iniciando os contêineres..."
                        docker compose up -d --remove-orphans
                        
                        echo "Deploy finalizado com sucesso!"

EOF
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline CI/CD executada com sucesso!'
        }
        failure {
            echo 'A Pipeline falhou.'
        }
    }
}

