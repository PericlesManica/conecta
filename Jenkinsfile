pipeline {
    agent any

    stages {
        stage('Clonar Repositorio') {
            steps {
                git 'https://github.com/PericlesManica/conecta.git'
            }
        }

        stage('Build Maven') {
            steps {
                script {
                    // Comando mvn do Windows, ajustar o caminho conforme necessário
                    bat 'mvn clean package'
                }
            }
        }

        stage('Construir Imagem Docker') {
            steps {
                script {
                    // Use o comando docker do Windows para construir a imagem
                    bat 'docker build -t listener-api:latest .'
                }
            }
        }

        stage('Deploy no Docker Host') {
    steps {
        // Parar e remover um contêiner existente no Docker host
        script {
            bat 'docker stop listener-api 2>NUL || exit 0' // Suprime a mensagem de erro se o contêiner não existir
            bat 'docker rm listener-api 2>NUL || exit 0'   // Suprime a mensagem de erro se o contêiner não existir
        }

        // Executa o contêiner com a nova imagem no Docker host
        script {
            bat 'docker run -d -p 8083:8083 --name listener-api listener-api:latest'
        }
    }
}

    post {
        success {
            echo 'Pipeline bem-sucedido! Executar ações adicionais aqui, se necessário.'
        }

        failure {
            echo 'Pipeline falhou! Executar ações adicionais aqui, se necessário.'
        }
    }
}