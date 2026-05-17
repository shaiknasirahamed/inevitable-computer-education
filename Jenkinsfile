pipeline {
    agent any

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/shaiknasirahamed/inevitable-computer-education.git'
            }
        }

        stage('Build Docker Image') {
    steps {
        script {
            env.IMAGE_TAG = "${env.BUILD_NUMBER}"
        }
        sh "docker build -t inevitable-computer-education:${IMAGE_TAG} ."
    }
}

        stage('Push Docker Image') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
                docker tag inevitable-computer-education:$IMAGE_TAG $DOCKER_USER/inevitbale-computer-eucation:$IMAGE_TAG
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push $DOCKER_USER/inevitable-computer-education:$IMAGE_TAG
            '''
        }
    }
}



             stage('Deploy Container') {
    steps {
        sh 'docker rm -f student-app-container || true'
        sh 'docker run -d -p 3000:80 --name inevitable-computer  shaiknasirahamed/inevitable-computer-education:$IMAGE_TAG'
    }
}
    }
}
