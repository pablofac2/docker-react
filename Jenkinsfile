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
                sh 'chmod +x ./jenkins-scripts/build.sh' //da permisos de ejecución al archivo
                sh './jenkins-scripts/build.sh'
                //sh 'sudo docker build -t pablofac/docker-react -f Dockerfile.dev .'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the checked-out project using a script in the repo..';
                sh 'chmod +x ./jenkins-scripts/test.sh' //da permisos de ejecución al archivo
                sh './jenkins-scripts/test.sh'
                //sh exit (“1”);
            }
        }
    }
    post {  //similar a try catch
        always {
            echo 'This will always run'
            echo 'Cleaning Testing stuff'
            sh 'chmod +x ./jenkins-scripts/clean.sh' //da permisos de ejecución al archivo
            sh './jenkins-scripts/clean.sh'
            //cualquier falla acá detiene el script
        }
        success {
            echo 'Successful Test'
            echo 'Deploying to AWS'
            script {
                try {   //avoid stopping deployment if docker-compose.yml doesn't exist
                    sh 'sudo mv docker-compose.yml dockercompose.nouse'   //aws linux 2 Docker AMI executes docker-comose is present, so renaming it to build Dockerfile instead
                    sh 'sudo mv docker-compose.ml dockercompose.nouse2'  //para verificar si falla y sigue
                } catch (err) {
                    echo "no docker-compose.yml file"
                }
            }
            step([$class: 'AWSEBDeploymentBuilder', credentialId: 'AKIAUCFWKMQ3N6KN66XG', awsRegion: 'us-east-1', applicationName: 'docker-react', environmentName: 'Dockerreact-env', rootObject: '.', bucketName: 'elasticbeanstalk-us-east-1-279555236918', versionLabelFormat: '${GIT_COMMIT}-${BUILD_TAG}'])
        }
    }
}