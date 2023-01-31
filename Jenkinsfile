node {
    stage ('git'){
        git branch: 'dev', url: 'https://github.com/rajesh1218/mavenwithk8s.git'
    }
        stage('maven build') {
       sh 'mvn clean install'
    }
    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t rja-web-app .'
        sh 'docker image list'
        sh 'docker tag rja-web-app rajesh1218/rja-web-app:latest'
    }
    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u rajesh1218 -p $PASSWORD'
    }
    stage("Push Image to Docker Hub"){
        sh 'docker push rajesh1218/rja-web-app:latest'
    }
    stage('Deploy to k8s'){
        sh 'kubectl apply -f deploy.yml'
        sh 'kubectl apply -f service.yml'
    } 
}    
