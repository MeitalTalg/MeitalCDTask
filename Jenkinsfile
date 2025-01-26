pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                script {
                    echo "starting to build"
                    
                    sh ' sudo docker build -t simple-flask-app:latest .'
                    sh ' docker run -d -p 5000:5000 --name web simple-flask-app ' 
                }
            }
        }

    stage('test') {
        steps {
            script {
                echo " test "
                sh ' docker exec web curl http://localhost:5000 '
            }
        }
    }

        stage('deploy') {
            steps {
                script {
                    echo "deploy"
                    sh ' aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 992382545251.dkr.ecr.us-east-1.amazonaws.com '               
                    sh '  docker build -t meital-repo . '
                    sh ' docker tag meital-repo:latest 992382545251.dkr.ecr.us-east-1.amazonaws.com/meital-repo:latest'
                    sh ' docker push 992382545251.dkr.ecr.us-east-1.amazonaws.com/meital-repo:latest ' 
                }
            }
        }
      
    }
    post { 
        always { 
            echo 'Delete the countiner'
            sh 'docker rm -f web '            
        }
     }
}  

