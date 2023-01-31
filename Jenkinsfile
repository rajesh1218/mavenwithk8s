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
    stage("SSH Into k8s Server") {
        def remote = [:]
        remote.name = 'K8S master'
        remote.host = '192.168.29.142'
        remote.user = 'osboxes'
        remote.password = 'Anki@1218'
        remote.allowAnyHosts = true
        
        stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
            sshPut remote: remote, from: 'deploy.yml', into: '.'
            sshPut remote: remote, from: 'service.yml', into: '.'
        }
        stage('Deploy spring boot to k8s') {
          sshCommand remote: remote, command: "kubectl apply -f deploy.yml"
          sshCommand remote: remote, command: "kubectl apply -f service.yml"  
        }
    }
}
