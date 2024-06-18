pipeline{
    agent any
    environment{
        registry="dporras13/dado_eoi"
        registryCredentials="dockerhub"
	project="dado_eoi"
	projectVersion="1.0"
	repository="https://github.com/dporras13/dporrasEOI.git"
	repositoryCredentials="github"
    }
    stages{
    	stage ('Clean Workspace') {
	    steps{
                cleanWs()
            }	
        }
        stage('Checkout code'){
            steps{
                script{
                    git branch: 'main',
                        credentialsId: repositoryCredentials,
                        url: repository
                }
            }
        }
        stage('Build del docker'){
            steps{
                script{
                    dockerImage= docker.build registry
                }
            }
        }
        stage('Test'){
            steps{
                script{
                    try{
                        sh 'docker run --name $project -e "LENGTH=20" $registry'
                    }finally{
                        sh 'docker rm $project'
                    }

                }
            }
        }
        stage('Deploy de la imagen'){
            steps{
                script{
                    docker.withRegistry('',registryCredentials ){
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Cleaning Up'){
            steps{
                script{
                    sh 'docker rmi $registry'
                }
            }
        }

    }
    post{
        failure{
            echo 'El pipeline ha fallado'
        }
    }

}
