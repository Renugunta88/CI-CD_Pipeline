pipeline {
    agent any
    environment {
        PROJECT_ID = 'PROJECT-ID'
        CLUSTER_NAME = 'CLUSTER-NAME'
        LOCATION = 'CLUSTER-LOCATION'
        CREDENTIALS_ID = 'gke'
    }
    stages {
        stage('Checkout Source') {
      steps {
        git url:'https://github.com/Renugunta88/CI-CD_Pipeline.git', branch:'main'
      }
    }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("renugunta88/node:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '1f98bb7f-b2f4-4f95-86cb-b33149953639') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to App') {
            steps {
        script {
          //kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
          sh 'sed -i \'s/BUILDNUMBER/\'$BUILD_ID\'/g\' deployment.yml'
          sh 'cat deployment.yml'
          sh 'kubectl apply -f deployment.yml'
        }
      }
        }
    }    
}