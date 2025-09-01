pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'gardinfernando'
    }

    stages {
        stage('Cleanup') {
            steps {
                echo 'Limpando o workspace...'
                // O -a remove todas as imagens não utilizadas, não apenas as pendentes
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
                    // Aponta para o Dockerfile do serviço WEB
                    docker.build("${DOCKERHUB_USER}/atividade2-web:latest", "-f ./web/Dockerfile.web ./web")
                    
                    // --- CORREÇÃO APLICADA AQUI ---
                    // Aponta para o Dockerfile do serviço DB
                    docker.build("${DOCKERHUB_USER}/atividade2-db:latest", "-f ./db/Dockerfile.mysql ./db")
                }
            }
        }
        stage('Entrega (Delivery)') {
            steps {
                echo 'Enviando imagens para o Docker Hub...'
                // Usando a credencial 'Docker_hub' que você já configurou
                withDockerRegistry(credentialsId: 'Docker_hub') {
                    docker.image("${DOCKERHUB_USER}/atividade2-web:latest").push()
                    docker.image("${DOCKERHUB_USER}/atividade2-db:latest").push()
                }
            }
        }
        // --- ESTÁGIO FINAL DE IMPLANTAÇÃO ---
        stage('Deploy (Implantação)') {
            steps {
                echo 'Iniciando o deploy no servidor de destino...'
                // Use o ID da sua credencial SSH cadastrada no Jenkins
                sshagent(credentials: ['sua-credencial-ssh-aqui']) { 
                    // Substitua 'usuario@ip-do-servidor' pelos dados reais do seu servidor
                    sh '''
                        ssh -o StrictHostKeyChecking=no usuario@ip-do-servidor <<EOF

                        # Garante que o repositório com o docker-compose.yml exista
                        if [ ! -d "atividade2" ]; then
                            git clone https://github.com/Gardinfe/atividade2.git
                        fi
                        cd atividade2
                        git pull

                        # Baixa as imagens mais recentes construídas pelo Jenkins
                        echo "Baixando novas versões das imagens..."
                        docker-compose pull
                        
                        # Sobe os serviços com as novas imagens em modo detached (-d)
                        echo "Iniciando os contêineres..."
                        docker-compose up -d --remove-orphans
                        
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

