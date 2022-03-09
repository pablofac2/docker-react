# CI Example Setup

This is a CI/CD example using Jenkins and AWS.

## Requirements:

AWS user, this example will use only the free tier
Jenkins user
Github user
Docker installed on your machine (used to run Jenkins and the Application tests phase)

## Config files explanation

Dockerfile creates an image to run our application.
Dockerfile.dev creates an image to run the tests needed before the automated deployment.
Jenkinsfile and jenkins-scripts contains the Jenkins Pipeline for the CI/CD.

## AWS Elastic Beanstalk Creation

First we need to log into our Amazon Web Services account and create a new Elastic Beanstalk Application, selecting Docker in platform.

Then we need the API keys to later deploy our application:
- Go to IAM Identity Access Management, Users, new User, programatic access
- Add existing policies: AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy, EC2 Full Access and S3 Full Access
- Take note of the Access Key ID and Secret Access Key

## GitHub repo pull

Pull the repo to your local work folder

## Jenkins Installation

We will be runing Jenkins in a Docker container.
Also, in order to let Jenkins run the testing stage in the Dockerfile.dev, we need to expose Docker sockets to the Jenkins container, in order to do so refer to this repo:
https://github.com/pablofac2/jenkins-withdocker.git

Ones Jenkins container up and running, access it on localhost:8080

Install and configure the following Plugin:
- AWSEB Deployment: This plugin allows you to deploy into AWS Elastic Beanstalk by Packaging, Creating a new Application Version, and Updating an Environment
- Open Jenkins: Jenkins > Credentials > System > Global credentials (unrestricted) > Add Credentials
- Give to the credentials (ID) the name "aws-credential-id" otherwise needs to be changed in Jenkinsfile
- Type created AWS Access Key ID and Secret Access Key

Install and configure the following Plugin:
- GitHub plugin: lets jenkins know when a new push to the GitHub repo occurs, initiating the Automated Testing and Deployment.

Finally we need to create a new Jenkins Job as Pipeline:
- Give it a name
- Enable "GitHub hook trigger for GITScm polling"
- In Pipeline Definition select "Pipeline script from SCM", Guit, paste your GitHub Pero URL, Main Branch, etc
- Save

## Jenkinsfile customization

Update the Jenkinsfile with your own AWS and Github Repo information.

## Pushing Changes to GitHub

Now if we push to the Main:
- On your local Repo Folder
- git init
- git add .
- git remote add origin {your repo URL}
- git branch -M main
- git commit -m "first commit"
- git push -u origin main

## Check the Deployment on Jenkins and AWS Elastic Beanstalk



# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
