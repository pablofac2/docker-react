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
                //sh 'chmod +x ./jenkins-scripts/build.sh' //da permisos de ejecución al archivo
                //sh './jenkins-scripts/build.sh'
                //sh 'sudo docker build -t pablofac/docker-react -f Dockerfile.dev .'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the checked-out project using a script in the repo..';
                //sh 'chmod +x ./jenkins-scripts/test.sh' //da permisos de ejecución al archivo
                //sh './jenkins-scripts/test.sh'
                //sh exit (“1”);		//to generate test error
            }
        }
    }
    post {  //similar a try catch
        always {
            echo 'This will always run'
            echo 'Cleaning Testing stuff'
            //sh 'chmod +x ./jenkins-scripts/clean.sh' //da permisos de ejecución al archivo
            //sh './jenkins-scripts/clean.sh'
            //any failure here also stops the script
        }
        success {
            echo 'Successful Test'
            echo 'Deploying to AWS'
            echo "${env.WORKSPACE}"
            echo "${env.JOB_NAME}"
            echo "${env.BUILD_TAG}"
            echo "${env.BUILD_NUMBER}"
            script {
                try {   //avoid stopping deployment if docker-compose.yml doesn't exist
                    sh 'sudo mv docker-compose.yml dockercompose.dontuse'   //aws linux 2 Docker AMI executes docker-comose is present, so renaming it to build Dockerfile instead
                } catch (err) {
                    echo "no docker-compose.yml file"
                }
            }
            //sh "sudo zip -r aws-deploy-${env.BUILD_NUMBER}.zip ."
            script {
                zip dir: "", zipFile: "aws-deploy-${env.BUILD_NUMBER}.zip", exclude: "**/.git/**/*,**/node_modules/**/*,*.zip"
            }
            step([$class: 'AWSEBDeploymentBuilder', credentialId: 'aws-credential-id', awsRegion: 'us-east-1', applicationName: 'docker-react', environmentName: 'Dockerreact-env', rootObject: "aws-deploy-${env.BUILD_NUMBER}.zip", bucketName: 'elasticbeanstalk-us-east-1-279555236918', versionLabelFormat: 'jenkins-aws-deploy-${BUILD_NUMBER}'])
            //Root Object is supposed to be workspace-relative, not absolute like /tmp/.
            //versionLabelFormat: '${BUILD_TAG}'
            // 'jenkins-aws-deploy-7'
            sh "sudo rm aws-deploy-${env.BUILD_NUMBER}.zip"
        }
    }
}