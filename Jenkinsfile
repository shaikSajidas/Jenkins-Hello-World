properties([pipelineTriggers([githubPush()])])

pipeline {
    environment {
        // name of the image without tag
        dockerRepo = "sajidan"
        dockerCredentials = 'docker_hub'
        dockerImageVersioned = ""
        dockerImageLatest = ""
    }

    agent any

    stages {
        /* checkout repo */
        stage('Checkout SCM') {
            steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'master']],
                 userRemoteConfigs: [[
                    url: 'https://github.com/shaikSajidas/Jenkins-Hello-World.git',
                    credentialsId: '',
                 ]]
                ])
            }
        }
    }
        stage("Building docker image"){
            steps{
                script{
                    dockerImageVersioned = docker.build dockerRepo + ":$BUILD_NUMBER"
                    dockerImageLatest = docker.build dockerRepo + ":latest"
                }
            }
        }
        stage("Pushing image to registry"){
            steps{
                script{
                    
                    withCredentials([usernamePassword(credentialsId: 'sajidan', passwordVariable: 'docker_password', usernameVariable: 'docker_username')]) {
                    sh "docker login -u $docker_username -p $docker_password"
                    sh "docker image push  sajidan/helloworld:latest"
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $dockerRepo:$BUILD_NUMBER"
            }
        }
    }

    /* Cleanup workspace */
    post {
       always {
           deleteDir()
       }
   }
}
