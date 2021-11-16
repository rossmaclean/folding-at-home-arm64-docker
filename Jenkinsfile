pipeline {
    agent any

    environment {
        DOCKERHUB_TOKEN = credentials('DOCKERHUB_TOKEN')
        SHORT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rossmaclean/folding-at-home-arm64-docker.git']]]
            }
        }
        stage('Docker Login') {
            steps {
                sh 'docker login --username rossmaclean --password $DOCKERHUB_TOKEN'
            }
        }
        stage('Docker Buildx') {
            steps {
                sh 'docker buildx build --platform linux/arm64 -t rossmaclean/fahclient-arm64:$SHORT_COMMIT .'
            }
        }
        stage('Docker Push') {
            steps {
                sh 'docker push rossmaclean/fahclient-arm64:$SHORT_COMMIT'
            }
        }
        stage('Docker Lgout') {
            steps {
                sh 'docker logout'
            }
        }
    }
}