pipeline{
    agent { label "master"}
    environment{
        SHELL= "/bin/bash"
    }
    stages{
        stage("Update Sonarqube report"){
            steps{
                sh "export PATH=$PATH:/var/lib/sonar-scanner/bin"
                sh "/var/lib/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=website-app2 -Dsonar.sources=. -Dsonar.host.url=http://172.16.107.148:9000 -Dsonar.login=sqp_dccbda5e875c263dadba6b275460f7c0268ff2c9 -Dsonar.coverage.exclusions=**/*"
            }
        }
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

