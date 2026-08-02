pipeline {
    agent any
    
    tools{
        jdk "JDK21"
        maven "maven3"
    }    

    stages {
        stage('Git Clone') {
            steps {
               git branch: 'main', url: url: 'https://github.com/sarathsankar080/project3-source.git'
            }
        }
        stage('Maven Build'){
            steps{
             sh 'mvn clean package'
            }
        }
        stage('Docker Image'){
            steps{
             sh 'docker build -t sarath1221/products-api:latest .'
            }
        }
        stage('Docker Image Push') {
            steps {
             withCredentials([usernamePassword(
             credentialsId: 'dockerhub',
             usernameVariable: 'DOCKER_USER',
             passwordVariable: 'DOCKER_PASS'
        )]) {
            sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
            sh 'docker push sarath1221/products-api:latest'
            }
          }
        }
        stage('k8s deployment'){
            steps{
             sh 'kubectl apply -f Deployment.yml'
            }
        }        
    }
}
