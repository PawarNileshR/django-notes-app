pipeline {
    
    agent any 
    
    stages {
        stage("code") {
            steps {
                echo "code clone"
                git url:"https://github.com/PawarNileshR/django-notes-app.git", branch: "main"
            }
        }
        
        stage("build") {
            steps {
                echo "build the code"
                sh "docker build -t node-app ."
            }
        }
        
        stage("push to dockerHub") {
            steps {
                echo "push the code on dockerHub"
                
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass", usernameVariable:"dockerHubuser")]) {
                sh "docker tag node-app ${env.dockerHubuser}/node-app:latest" 
                sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubuser}/node-app:latest"
                }
            }
        }  
        
        stage("Deploy") {
            steps {
                echo "deploy the App"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
