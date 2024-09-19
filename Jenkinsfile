podTemplate(containers: [
    containerTemplate(
        name: 'docker',
        image: 'docker:dind',
        command: 'sleep',
        args: '99d',
        ttyEnabled: true,
        privileged: true
    )
],
volumes: [
    hostPathVolume(
        hostPath: '/var/run/docker.sock',
        mountPath: '/var/run/docker.sock'
    )
]) {

    pipeline {
        agent {
            label POD_LABEL
        }

        environment {
            IMAGE_TAG = "O.${BUILD_ID}"
            IMAGE_NAME = "simple-python-flask"
        }

        // Checkout do código
        container('docker') {
            git 'https://github.com/alissonoliveira0607/simple-python-flask.git'
        }

        // Build da imagem
        container('docker') {
            sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
        }

        // Execução dos testes
        container('docker') {
            sh """
                docker run -d -ti --rm --name ${IMAGE_NAME}-${IMAGE_TAG} ${IMAGE_NAME}:${IMAGE_TAG}
                docker exec ${IMAGE_NAME}-${IMAGE_TAG} nosetests --with-xunit --with-coverage --cover-package=project test_users.py
            """
        }

        post {
            success {
                echo "Pipeline executada com sucesso"
            }
            failure {
                echo "Pipeline falhou"
            }
            cleanup {
                container('docker') {
                    sh "docker stop ${IMAGE_NAME}-${IMAGE_TAG}"
                }
            }
        }
    }
}
