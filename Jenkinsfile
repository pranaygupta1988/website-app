pipeline{
    agent { label "master"}
    environment{
        SHELL= "/bin/bash"
    }
    stages{
        stage("Build the image"){
         steps{
           sh "docker image build -t pranaygupta1988/website-app:${BUILD_NUMBER} ."
            }
        }
        stage("Push the image"){
            steps{
                withCredentials([string(credentialsId: 'DOCKER_HUB_TOKEN', variable: 'DOCKER_HUB_TOKEN')]) 
                {
                    sh "echo $DOCKER_HUB_TOKEN | docker login -u pranaygupta1988 --password-stdin"
                }

                sh "docker image push pranaygupta1988/website-app:${BUILD_NUMBER}"
            }
        }
        stage("Update Deployent File"){
            steps{
                sh "sed -i 's/website-app:.*/website-app:${BUILD_NUMBER}/g' dep1.yaml"
            }
        }
        stage("Push Deployment File"){
            steps{
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) 
                {
                    sh "git config --global user.email 'pranaygupta2022@gmail.com'"
                    sh "git config --global user.name 'Pranay G Gupta'"
                    sh "git add dep1.yaml"
                    sh "git commit -m 'Deployment file updated with build number ${BUILD_NUMBER}'"
                    sh "git push https://${GITHUB_TOKEN}@github.com/pranaygupta1988/website-app HEAD:main"
                }
            }
        }
    }
        
}

