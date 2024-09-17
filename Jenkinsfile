pipeline {
    agent any
    stages {
        // Estágio de build da aplicação
        stage ("Build") {
            steps {
                sh "docker build -t simple-python-flask ."
            }
        }
    }
}
