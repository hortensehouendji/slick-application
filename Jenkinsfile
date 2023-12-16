pipeline{
    agent any
    environment {
        GIT_BRANCH = 'main'
        DOCKER_REPOSITORY = 'hortensehouendji/slick-application'
        DOCKER_CREDENTIALS = credentials('dockerhubCreds')
    }
    
    stages{
        stage ('Source'){
            steps{
                sh 'echo pulling the code from github'
                git branch: 'main', credentialsId: 'GitCreds', url: 'https://github.com/hortensehouendji/slick-application.git'
            }
        }

        stage ('Build'){
            steps{
                sh 'echo building the Dockerfile'
                // building the docker image of our application
                //sh 'docker build -t $DOCKER_REPOSITORY:v${env.BUILD_NUMBER} .'
                sh 'docker build -t $DOCKER_REPOSITORY:v$BUILD_NUMBER .'
            }
        }

        stage ('Test'){
            steps{
                sh 'echo Testing the container image'
                sh 'echo Creating a container from the docker image for testing purposes'
                sh 'docker run --name "slickapp-container-$BUILD_NUMBER" -t -d -p 5000:80 $DOCKER_REPOSITORY:v$BUILD_NUMBER'
            }
        }

        stage ('Manual approval'){
            steps{
                sh 'echo waiting for manual approval'
                input 'revision is needed'
            }
        }

        stage ('Deploy to Dockerhub'){
            steps{
                sh 'echo loging into dockerhub'
                sh 'docker login -u$DOCKER_CREDENTIALS_USR -p$DOCKER_CREDENTIALS_PSW'
                sh 'echo dockerhub login success'

                sh 'echo deploy the container on dockerhub'
                sh 'docker push $DOCKER_REPOSITORY:v$BUILD_NUMBER'

                sh 'echo the image of the following container was successfully deployed on github: $DOCKER_REPOSITORY:v$BUILD_NUMBER'
            }
        }

        stage ('Deploy to k8s'){
            steps{
                sh 'echo testing the connectivity to the cluster' 
                sh 'kubectl get nodes'
            }
        }



    }
}