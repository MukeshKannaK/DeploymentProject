pipeline {
    agent any

    stages {
        stage('Pull Code From GitHub') {
            steps {
                git url: 'https://github.com/MukeshKannaK/DeploymentProject.git', branch: 'master'
            }
        }

        stage('Build the Docker image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t img /var/lib/jenkins/workspace/Final'
                    
                    // Tag the Docker image
                    sh "docker tag img mukeshkannak/14aug:latest"
                    sh "docker tag img mukeshkannak/14aug:${BUILD_NUMBER}"
                }
            }
        }

        stage('Push the Docker image') {
            steps {
                script {
                    // Log in to Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'key', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                    }

                    // Push the Docker image to the repository
                    sh 'docker push mukeshkannak/14aug:latest'
                    sh 'docker push mukeshkannak/14aug:${BUILD_NUMBER}'
                }
            }
        }

        stage('Deploy on Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes configuration
                    sh 'echo "123" | sudo -S kubectl apply -f /var/lib/jenkins/workspace/Final/pod.yaml'
                    
                    // Restart the Kubernetes deployment
                    sh 'echo "123" | sudo -S kubectl rollout restart deployment/loadbalancer-pod'
                }
            }
        }
    }
}
