pipeline{
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage("Build Maven Project"){
            steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/thedeveloperme10/hw2-swe-645']])
                sh 'mvn clean install'
            }
        }
        stage("Build Docker Image"){
            steps{
                script{
                    sh 'docker build -t vsomasun/swe-642-cicd .'
                }
            }
        }
        stage("Push Docker Image to DockerHub"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u vsomasun -p ${dockerhubpwd}'
                        sh 'docker push vsomasun/swe-642-cicd'
                    }
                }
            }
        }
        stage("Deploy to kubernetes"){
            steps{
                script{
                    env.KUBECONFIG = '/home/ubuntu/.kube/config'
                    sh("kubectl --kubeconfig=${env.KUBECONFIG} set image deployment/swe-645-hw2 container-0=vsomasun/swe-642-cicd:latest -n default")
                }
            }
        }
    }
}
