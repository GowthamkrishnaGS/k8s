pipeline {
    agent any
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/Iam-mithran/weekend-proj'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t pyimage /var/lib/jenkins/workspace/weekend-proj'
                sh 'sudo docker tag pyimage iammithran/pyimage:latest'
                sh 'sudo docker tag pyimage iammithran/pyimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push iammithran/pyimage:latest'
                sh 'sudo docker image push iammithran/pyimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /var/lib/jenkins/workspace/weekend-proj/pod.yaml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}