pipeline {
    agent any

    stages {

         stage('Download from Git') {
            steps {
               checkout scmGit(
                 branches: [[name: '*/main']],
                 userRemoteConfigs: [[
                   url: 'https://github.com/satyanarayana-24/frontend_dep_docker_lakshman.git'
                 ]]
               )
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t tes-institute:${BUILD_NUMBER} ."
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop tes-app || true
                docker rm tes-app || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d -p 8081:8080 \
                --name tes-app tes-institute:${BUILD_NUMBER}
                '''
            }
        }

        // stage('Cleanup Old Docker Images') {
        //     steps {
        //         sh 'docker image prune -f'
        //     }
        // }
        stage('Cleanup Old Docker Images') {
           steps {
            sh '''
            docker image prune -f
            docker images tes-institute -q | tail -n +3 | xargs -r docker rmi
            '''
                }
        }
    }
}
