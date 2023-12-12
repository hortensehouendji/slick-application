pipeline{
    agent any
    
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
            }
        }

        stage ('Test'){
            steps{
                sh 'echo Testing the container image'
            }
        }
    }
}