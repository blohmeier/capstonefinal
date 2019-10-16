pipeline {
    environment {
        registry = "blohmeier/capstone-final"
        registryCredential = "dockerhub"
    }
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Linting the HTML') {
            steps {
                 sh 'tidy -q -e *.html'
             }
	}
        stage('Building the Docker Image') {
            steps {
			    sh 'docker build . -t blohmeier/capstone-final'
	    }
         }
         stage('Push Docker Image') {
             when {
                branch 'master'
            }
            steps {
		    script {
			    withDockerRegistry( credentialsId: "dockerhub") {
				    sh 'docker tag blohmeier/capstone-final:latest blohmeier/capstone-final'
				    sh 'docker push blohmeier/capstone-final'
			    }
		    }
	    }
	 }
	 stage('Remove old Docker Container') {
      		steps {
			sh 'docker container rm capstone-final -f'
		}
	 }
	 stage('Build Docker Container') {
		 steps {
			 sh 'docker run --name capstone-final -d -p 8000:80 blohmeier/capstone-final'
		 }
	 }
    }
}
