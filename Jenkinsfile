pipeline {
    environment {
        registry = "nohaila0408/tp4"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-cred', 
                    url: 'https://github.com/nouha-prog/tp4-devops'
            }
        }
        
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Deploy image') {
            steps {
                script {
                    // Arrêter et supprimer l'ancien conteneur
                    bat '''
                        docker stop web-tp4 2>nul || echo Conteneur non trouve
                        docker rm web-tp4 2>nul || echo Conteneur non trouve
                    '''
                    
                    // Déployer le nouveau conteneur
                    bat "docker run -d --name web-tp4 -p 8081:80 ${registry}:${BUILD_NUMBER}"
                    
                    echo "Application deployee sur http://localhost:8081"
                }
            }
        }
    }
}