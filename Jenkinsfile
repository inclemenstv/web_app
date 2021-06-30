pipeline {
    agent any
    triggers{ pollSCM('* * * * *') }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
    }
    environment {
        DOCKERHUB = credentials("${env.DockerHub_ID}")
    }
    stages {
        stage('1-Docker login') {
            steps {
                echo "Docker login..."
                sh '''
                   docker login 192.168.20.10:5000 -u admin -p admin
                '''
            }
        }
        stage('2-Create docker image') {
            steps {
                echo "Start Build Backend and Frontend images..."
                sh '''
                   cd backend
                   docker build -t 192.168.20.10:5000/backend:latest .
                   cd ..
                   cd frontend
                   docker build -t 192.168.20.10:5000/frontend:latest .
                '''
                echo "Building......."
                echo "End of Stage Build..."
            }
        }
         stage('3-Push docker images') {
            steps {
                echo "Start push image..."
                sh '''
                   docker push 192.168.20.10:5000/backend:latest
                   docker push 192.168.20.10:5000/frontend:latest
                '''
            }
        }
        stage('4-Remove unused docker images') {
            steps {
                echo "Start remove image..."
                sh '''
                   docker image prune -f
                '''
            }
        }
        stage('5-Deploy') {
            steps {
                echo "Start of Stage Deploy..."
                echo "Deploying..."
                script {
                sh """
                    sudo apt-get install sshpass -y
                    ssh -tt -i /var/lib/jenkins/.ssh/id_rsa -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no vagrant@$192.168.50.10<< EOF
                    sudo docker login 192.168.20.10:5000 -u admin -p admin
                    sudo docker stop web_app
                    sudo docker rm web_app
                    sudo docker rmi $DOCKERHUB_USR/$DockerHub_Repository:latest
                    sudo docker run -d -p 8080:80 --name web_app --restart unless-stopped $DOCKERHUB_USR/$DockerHub_Repository:latest
                    exit
                EOF"""
                }
                echo "End of Stage Deploy..."
            }
        }
        stage('6-Test') {
            steps {
                echo "Start of Stage Test..."
                echo "Testing..."
                sh '''
                    export result=`curl -L -s -o /dev/null -I -w "%{http_code}" http://$DEPLOY_HOST:8080 | grep 200`
                    if [ "$result" = "200" ]
                    then
                        echo "Test Passed"
                    else
                        echo "Test Failed"
                    fi
                '''
                echo "End of Stage Test..."
            }
        }
    }
}