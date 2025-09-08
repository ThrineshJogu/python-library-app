# this is a microservices project suing python code


Clone the repo 

docker build -t dbimage database/ <br />
docker build -t authimage auth/ <br />
docker build -t bookimage book/ <br />
docker build -t borrowimage borrow/ <br />
docker build -t appimage . <br /> <br /> <br /> <br />


docker network create mynet <br /> <br /> <br /> <br />


docker run -d --name db -p 3306:3306 --network mynet dbimage <br />
docker run -d --name auth_service --network mynet  -p 5001:5001 authimage <br />
docker run -d --name book_service -p 5002:5002 --network mynet bookimage <br />
docker run -d --name borrow_service  -p 5003:5003 --network mynet borrowimage <br />
docker run -d --name frontend  -p 5000:5000 --network mynet  appimage <br /> 

______________________________________________________________________________________________________________________________

jenkins pipeline 


pipeline {
    agent any
    stages {
        stage('code') {
            steps {
                git branch: 'main', url: 'https://github.com/ThrineshJogu/python-library-app.git'
            }
        }
        stage('image'){
            steps{
                sh'''
                docker build -t dbimage database/
                docker build -t authimage auth/
                docker build -t bookimage book/
                docker build -t borrowimage borrow/
                docker build -t appimage .
                docker network create mynet
                '''
            }
        }
        stage('container'){
            steps{
            sh'''
            docker run -d --name db -p 3306:3306 --network mynet dbimage
            docker run -itd --name auth_service --network mynet -p 5001:5001 authimage
            docker run -itd --name book_service -p 5002:5002 --network mynet bookimage
            docker run -itd --name borrow_service -p 5003:5003 --network mynet borrowimage
            docker run -itd --name frontend -p 5000:5000 --network mynet appimage
            '''
            }
        }
    }
    post{
        always{
            echo "pipeline completed"
        }
    }
}


