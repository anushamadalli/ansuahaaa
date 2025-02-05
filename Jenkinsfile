pipeline {
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('Check and remove container'){
            steps{
                script{
                    def containerExists = sh(script: "docker ps -q -f name=anusha", returnStdout: true).trim()
                    if (containerExists) {
                    sh "docker stop anusha"
                    sh "docker rm anusha"
                    }
                }
            }
        }
        stage('Build package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Create image'){
            steps{
                sh 'sudo docker build -t app /var/lib/jenkins/workspace/demo/'
            }
        }
        stage('Assign tag'){
            steps{
                sh 'docker tag app anushamadalli/app'
            }
        }
        stage('Push to dockerhub'){
            steps{
                sh 'echo "@docker#123" | docker login -u "anushamadalli" --password-stdin'
                sh 'docker push anushamadalli/app'
            }
        }
        stage('Remove images'){
            steps{
                sh 'docker rmi -f $(docker images -q)'
            }
        }
        stage('Pull image from DockerHub'){
            steps{
                sh 'docker pull anushamadalli/app'
            }
        }
        stage('Run a container'){
            steps{
                sh 'docker run -it -d --name chandu -p 8081:8080 anushamadalli/app'
            }
        }
    }
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            sh 'docker rm -f anusha'
        }
        always{
            echo 'Deployed'
        }
    }

}