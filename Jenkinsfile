pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                echo 'Limpando o workspace...'
                // O Jenkins ja faz uma limpeza basica, mas aqui podemos remover arquivos antigos ou imagens Docker.
                sh 'docker image prune -f || true' // Remove imagens Docker nao utilizadas
                deleteDir() // Limpa o diretorio de trabalho do Jenkins
            }
        }
        stage('Checkout') {
            steps {
                echo 'Baixando o código do GitHub...'
                // A acao de checkout baixa o codigo do repositorio configurado no job
                git url: 'https://github.com/Gardinfe/atividade2.git', branch: 'main'
            }
        }
        stage('Construção (Build)') {
            steps {
                echo 'Iniciando a construcao das imagens Docker...'
                script {
                    // Constroi a imagem Docker para a aplicacao web
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
