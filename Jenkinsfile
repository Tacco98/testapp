@Library("Shared") _
pipeline{
    
    agent { label "tacco" }
    stages{
        stage("Code"){
        steps{
            script{
                clone("https://github.com/Tacco98/testapp.git","main")
            }
        }
    }
    stage("Build"){
        steps{
            script{
                dockerbuild("notesapp","latest")
            }
        }
    }
    stage("Push"){
        steps{
            echo "Pushing the code"
            withCredentials([usernamePassword(
                credentialsId:"dockerCred",
                usernameVariable:"dockerHubUser",
                passwordVariable:"dockerHubPass")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}"
                    sh "docker image tag notesapp:latest ${env.dockerHubUser}/notesapp:latest"
                    sh "docker push ${env.dockerHubUser}/notesapp:latest"
                }
        }
    }
    stage("Deploy"){
        steps{
            echo "Deploying the code"
            sh "docker compose up -d"
        }
    }
    }
}
