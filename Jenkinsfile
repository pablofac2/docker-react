pipeline {
    agent any

    stages {
        stage('Git-Checkout') {
            steps {
                echo 'Checking out from Git repo';
                git branch: 'main', url: 'https://github.com/pablofac2/docker-react.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the checked-out project using a script in the repo..';
                sh 'chmod +x ./jenkins-scripts/build.sh'
                //sh 'sudo docker build -t pablofac/docker-react -f Dockerfile.dev .'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the checked-out project using a script in the repo..';
                sh 'chmod +x ./jenkins-scripts/test.sh'
                //sh exit (“1”);
            }
        }
    }
}