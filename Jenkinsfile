pipeline {
  environment {
      registryCredential = "DOCKER_HUB_TOKEN"
      NGINX_REPO_CERT = credentials("NGINX_REPO_EVAL_CERT")
      NGINX_REPO_KEY = credentials("NGINX_REPO_EVAL_KEY")
   }
  agent any
  stages {
    stage('Building image') {
        steps{
            sh 'cat ${NGINX_REPO_CERT} > "nginx/nginx-repo.crt"'
            sh 'cat ${NGINX_REPO_KEY} > "nginx/nginx-repo.key"'
            sh 'docker-compose build'
        }
    } 
    stage('Dockerhub Approval Request') {
        steps {
            script {
                env.PUSH_TO_DOCKER_HUB = input message: 'User input required', 
                parameters: [choice(name: 'Push to Docker Hub', choices: 'no\nyes', description: 'Choose "yes" if you want to push this build')]
            }
        }
    }  
    stage('Pushing Image') {
        when {
            environment name: 'PUSH_TO_DOCKER_HUB', value: 'yes'
        }
        steps {
            echo "pushing to docker hub registry"
            script {
                docker.withRegistry('', registryCredential) {
                    sh 'docker-compose push'
                }
            }
        }
    } 
    stage('Deploy the Applications') {
        steps {
            sh 'docker-compose up -d'
        }
    }
	stage ("Dynamic Analysis - DAST with OWASP ZAP") {
        steps {
            echo "Waiting app to get ready!!"
            sleep(10)
            sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://192.168.255.203/taskManager -a -j || true"		
        }
  	} 
  } 
}