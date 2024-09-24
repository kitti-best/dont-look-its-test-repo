pipeline {
    agent none
    environment {
        APP_NAME = "simple api"
        IMAGE_NAME = "ghcr.io/kitti-best/SDPX-Jenkins-Container"
    }
    stages {
        stage("Unit test") {
            agent { label "vm2" }
            steps {
                sh "echo vm2 work!"
            }

        }
        stage("Robot test") {
            agent { label "vm2" }
            steps {
                withCredentials(
                    [usernamePassword(
                        credentialsId: "user_01",
                        passwordVariable: "gitPassword",
                        usernameVariable: "gitUsername",
                    )]
                ){
                    sh "docker login --username ${gitUsername} --password ${gitPassword}"
                    sh "docker build . -t ${IMAGE_NAME}"
                    sh "docker run -p 80:5000 ${IMAGE_NAME}"  // HOST:CONTAINER
                    sh "echo test using robot"
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }
        stage("Create Docker Container") {
            agent { label "vm3" }
            steps {
                withCredentials(
                    [usernamePassword(
                        credentialsId: "user_01",
                        passwordVariable: "gitPassword",
                        usernameVariable: "gitUsername",
                    )]
                ){
                    sh "docker login --username ${gitUsername} --password ${gitPassword}"
                    // sh "docker build . -t ${IMAGE_NAME}"
                    sh "docker run -p 80:5000 ${IMAGE_NAME}"  // HOST:CONTAINER
                    // sh "echo test using robot"
                    // sh "docker push ${IMAGE_NAME}"
                }
            }
        }
    }
}