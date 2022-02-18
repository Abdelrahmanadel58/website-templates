pipeline {
    agent { label 'jenkins-slave' }
    stages {
        stage('Build') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh """
                            docker build -t abdelrahman58/testlvl3 .
                            docker login -u '${USERNAME}' -p '${PASSWORD}'
                            docker push abdelrahman58/testlvl3
                        """
                    }

                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'k8s', variable: 'Secretfile')]) {
                        sh """
                            kubectl apply -f Backend.yaml --kubeconfig=$Secretfile
                        """
                    }
                }
                }
            }
        }
}
