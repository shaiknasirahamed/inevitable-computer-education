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
        sh "docker build -t student-app:${IMAGE_TAG} ."
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
                docker tag student-app:$IMAGE_TAG $DOCKER_USER/student-app:$IMAGE_TAG
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push $DOCKER_USER/student-app:$IMAGE_TAG
            '''
        }
    }
}


        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

             stage('Deploy Container') {
    steps {
        sh 'docker rm -f student-app-container || true'
        sh 'docker run -d -p 3000:3000 --name student-app-container shaiknasirahamed/inevitable-computer-education:$IMAGE_TAG'
    }
}
    }
}
