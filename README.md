## STEP 1: Build & Test the REACT APP

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `yarn build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify

## STEP 2: Containerization of the REACT APP using Docker 

This project has Containerization support using Docker: 

Note: you need to have Docker running locally for the commands to work. 
To get started with Docker: https://www.docker.com/get-started

In the project directory, you can run following to build a dev container:

### docker build -t k8s-react-frontend-dev -f Dockerfile.dev .

Then you can run following to run the  dev container:

### docker run -it --rm -p 3000:3000 k8s-react-frontend-dev

Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

Now, in the project directory, you can run following to build a production container with a nginx web server:

### docker build -t k8s-react-frontend .

Then you can run following to run the  dev container:

### docker run -it -p 3000:80 --rm k8s-react-frontend:latest

You can ignore the warnings in the console:
10-listen-on-ipv6-by-default.sh: error: /etc/nginx/conf.d/default.conf is not a file or does not exist

Open [http://localhost:3000](http://localhost:3000) to view it in the browser.


## STEP 3: Container Orchestration using Kubernates 

This project has reference yml files that can be use to deploy the Docker contaioners (from STEP 2) in Kubernates:

Note: you need to have Docker and Kubernates running locally for this script to work. 
One simple option is using the Kubernates engine that comes with Docker: https://www.docker.com/products/kubernetes

Under project directory, navigate to kubernates directory and review the two yml files: 

### react.frontend.deployment.yml
### react.frontend.service.yml

1. push the docker container images from STEP 2 to docker hub:
(Note: by default kubernates search for container images from Docker hub)

### docker tag k8s-react-frontend [your docker hub repository name]/k8s-react-frontend

2. deploy the docker container in a kubernated cluster using deployment:
(Note: you can deploy as pods as well but deployment is a preferred approach)

### kubectl apply -f react.frontend.deployment.yml 

3. verify the deployment was successful: 
### kubectl get deployment 

4. verify the pods with REACT container images are running: 
### kubectl get pods

5. verify a pod with REACT container is confgured & deployed correctly: 
### kubectl describe pod react-frontend-xxxxxxxxxx-xxxxx

6. verify the REACT app is working correctly: 
(Note: here we are simply binding a port from our local machine port 8080 to container port 80)
### kubectl port-forward deployment/react-frontend 8080:80  

Optionally, you can use port forward on the PODS as well.,
For a deployed application, you can use Kubernates Service: 
[https://kubernetes.io/docs/concepts/services-networking/service/] (https://kubernetes.io/docs/concepts/services-networking/service/)

### kubectl port-forward pod/react-frontend-xxxxxxxxxx-xxxxx 8080:80  

Open [http://localhost:8080](http://localhost:8080) to view the React app in the browser. 

You can exit the termical once done. 

7. Optionally, you can ssh into the container to verify the REACT app contents: 
### kubectl exec react-frontend-xxxxxxxxxx-xxxxx -it sh

navigate to /usr/share/nginx/html folder where you should see the contents from build folder of your REACT app (from STEP 1)

8. 