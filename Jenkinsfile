pipeline {
    agent any  // Define o agent / nó em que a pipeline será executada

    // Definição das variaveis que serão utilizadas na construção da imagem
    environment {
        IMAGE-TAG="O.${BUILD_ID}"
        IMAGE-NAME="simple-python-flask"
    }

    stages {
        // Stage de build da imagem para a aplicação
        stage("Build") {
            steps{
                sh "docker build -t ${IMAGE-NAME}:${IMAGE-TAG} ."
            }
        // Stage de testes para validar o build da imagem        
        stage("Teste"){
            steps{
                sh "docker run -d -ti --rm --name ${IMAGE-NAME}-${IMAGE-TAG} ${IMAGE-NAME}:${IMAGE-TAG}"

                sh "docker exec -ti ${IMAGE-NAME}-${IMAGE-TAG} nosetests --with-xunit --with-coverage --cover-package=project teste_users.py"
            }
        }
        
        }
    }
}
