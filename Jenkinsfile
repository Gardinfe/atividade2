pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                echo 'Limpando o workspace...'
                // O Jenkins já faz uma limpeza básica, mas aqui podemos remover arquivos antigos ou imagens Docker.
                sh 'docker image prune -f || true' // Remove imagens Docker não utilizadas
                deleteDir() // Limpa o diretório de trabalho do Jenkins
            }
        }
        stage('Checkout') {
            steps {
                echo 'Baixando o código do GitHub...'
                // A ação de checkout baixa o código do repositório configurado no job
                git url: 'https://github.com/Gardinfe/atividade2.git', branch: 'main'
            }
        }
        stage('Construção (Build)') {
            steps {
                echo 'Iniciando a construção das imagens Docker...'
                script {
                    // Constrói a imagem Docker para a aplicação web
                    echo 'Construindo imagem para o serviço WEB...'
                    def webImage = docker.build('atividade2-web', './web')
                    echo "Imagem ${webImage.id} construída com sucesso."

                    // Constrói a imagem Docker para o banco de dados
                    echo 'Construindo imagem para o serviço DB...'
                    def dbImage = docker.build('atividade2-db', './db')
                    echo "Imagem ${dbImage.id} construída com sucesso."
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
