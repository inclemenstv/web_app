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
                   docker login $DockerHub_Repository -u $DOCKERHUB_USR -p $DOCKERHUB_PSW
                '''
            }
        }
        stage('2-Create docker image') {
            steps {
                echo "Start Build Backend and Frontend images..."
                sh '''
                   cd backend
                   docker build -t $DockerHub_Repository/backend:latest .
                   cd ..
                   cd frontend
                   docker build -t $DockerHub_Repository/frontend:latest .
                '''
                echo "Building......."
                echo "End of Stage Build..."
            }
        }
         stage('3-Push docker images') {
            steps {
                echo "Start push image..."
                sh '''
                   docker push $DockerHub_Repository/backend:latest
                   docker push $DockerHub_Repository/frontend:latest
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

