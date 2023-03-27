pipeline{
    stage('git checkout'){
        git 'https://github.com/lavi1205/Jenkins-Docker-Project.git'
    }
    stage('docker build image'){
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID lavi1205/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID lavi1205/$JOB_NAME:latest'
    }
    stage('docker push image to docker hub'){
       withCredentials([string(credentialsId: 'Dockerpassword', variable: 'Dockerpass')]) {
        sh "docker login -u lavi1205 -p ${Dockerpass}"
        sh 'docker image push lavi1205/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image push lavi1205/$JOB_NAME:latest'
            
        sh 'docker image rm $JOB_NAME:v1.$BUILD_ID lavi1205/$JOB_NAME:v1.$BUILD_ID lavi1205/$JOB_NAME:lates'
       }
    }
    stage('Docker container deployment'){
        def docker_run = 'docker run -p 9000:80 -d --name scripted-pipeline-demo lavi1205/docker_webapp:latet'
        def docker_rm_container = 'docker rm -f scripted-pipeline-demo'
        def docker_rmi = 'docker rmi -f lavi1205/docker_webapp'
        sshagent(['Web_app_key']) {
        sh "ssh -o StrictHostKeyChecking=no root@172.31.31.224 ${docker_rm_container}"
        sh "ssh -o StrictHostKeyChecking=no root@172.31.31.224 ${docker_rmi}"
        sh "ssh -o StrictHostKeyChecking=no root@172.31.31.224 ${docker_run}"
}
    }
}