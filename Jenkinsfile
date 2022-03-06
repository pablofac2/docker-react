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
                sh './jenkins-scripts/build.sh'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the checked-out project using a script in the repo..';
                sh './jenkins-scripts/test.sh'
                //sh exit (“1”);
            }
        }
    }
}