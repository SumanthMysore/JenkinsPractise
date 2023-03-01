pipeline {
    
    agent any
    
    environment {
        be_image = 'ghcr.io/sumanthmysore/jenkins-bc26-be'
        fe_image = 'ghcr.io/sumanthmysore/jenkins-bc26-fe'
        be_port = 8090
        fe_port = 3000
        ghcr_credentials = credentials('ghcr_pat')
    }
    
    stages {
        
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'ghcr_pat', url: 'https://github.com/SumanthMysore/JenkinsPractise']])
            }
        }
        
        stage('Building docker image for backend') {
            steps {
                dir('backend/') {
                    sh 'docker build . -t ${be_image}'
                }
            }
        }
        
        stage('Pushing backend docker image') {
            steps {
                withCredentials([gitUsernamePassword(credentialsId: 'ghcr_pat', gitToolName: 'Default')]) {
                    sh 'docker push ${be_image}'
                }
            }
        }
        
        stage('Re-deploy the backend container') {
            steps {
                sshagent(credentials: ['ec2_keypair_pem']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.134.113.255 """
                            docker login ghcr.io --username $ghcr_credentials_USR --password $ghcr_credentials_PSW
                            docker stop be-container 2>/dev/null
                            docker rm be-container 2>/dev/null
                            docker rmi ${be_image} 2>/dev/null
                            docker run -p ${be_port}:${be_port} -d --name=be-container ${be_image}
                        """
                    '''
                }
            }
        }
        
        stage('Building docker image for frontend') {
            steps {
                dir('frontend/') {
                    sh 'docker build . -t ${fe_image}'
                }
            }
        }
        
        stage('Pushing frontend docker image') {
            steps {
                withCredentials([gitUsernamePassword(credentialsId: 'ghcr_pat', gitToolName: 'Default')]) {
                    sh 'docker push ${fe_image}'
                }
            }
        }
        
        stage('Re-deploy the frontend container') {
            steps {
                sshagent(credentials: ['ec2_keypair_pem']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.134.113.255 """
                            docker login ghcr.io --username $ghcr_credentials_USR --password $ghcr_credentials_PSW
                            docker stop fe-container 2>/dev/null
                            docker rm fe-container 2>/dev/null
                            docker rmi ${fe_image} 2>/dev/null
                            docker run -p ${fe_port}:${fe_port} -d --name=fe-container ${fe_image}
                        """
                    '''
                }
            }
        }
    }
}
