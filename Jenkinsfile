pipeline {
  environment {
    registryCredential = 'DOCKER_HUB_TOKEN'
    NGINX_REPO_CERT = credentials("NGINX_REPO_EVAL_CERT")
    NGINX_REPO_KEY = credentials("NGINX_REPO_EVAL_KEY")
   }
  agent any
  stages {
/*    stage('Building image') {
        steps{
//            sh "cp django.env.example django.env"
            sh 'cat ${NGINX_REPO_CERT} > "nginx/nginx-repo.crt"'
            sh 'cat ${NGINX_REPO_KEY} > "nginx/nginx-repo.key"'
            sh 'docker-compose build'
        }
    } */
    stage('Pushing Image') {
      steps{
        echo "pushing to docker hub registry"
        docker.withRegistry( '', registryCredential ) {
            dockerImage.push("akhng999/django-gunicorn-vn:latest")
            dockerImage.push('akhng999/nginx:nginxplus')
        }
       }
      }
    } 
  /*  stage('Deploy the Applications') {
        steps{
            sh 'docker-compose up -d'
        }
    }
	  stage ("Dynamic Analysis - DAST with OWASP ZAP") {
        steps {
            sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://192.168.255.203/ || true"
		    }
  	} */
  } 
}