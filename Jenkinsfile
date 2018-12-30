pipeline {
  environment {
    WORKSPACE=pwd()
    ETAG=''
    KOPS_STATE_STORE="s3://valuesource-kubernetes"
    CLUSTER_NAME="value-source-cloud.com"
    registry = "dcurrotto/dotnet-core-simple"
    registryCredential = 'DockerHubCredentials'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/dancurrotto/jenkins-to-dockerhub.git'
      }
    }
    stage('Build') {
       steps {
         sh 'echo building'
       }
    }
    stage('Test') {
      steps {
        sh 'echo testing'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy Cluster') {
            steps {
                
                sh 'echo $WORKSPACE'
                sh 'echo $AWS_ACCESS_KEY_ID'
              
                sh 'echo $PATH'            
                
               
                sh 'echo Configuring AWS...'
                sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
                sh 'aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
                sh 'aws configure set region us-east-2'
                sh 'aws configure set output json'   
                
                
                sh 'echo Tell kops where to find its config and state.'
               
                //********************************************************************

                ///Create:
                /*
                sh 'kops create -f $CLUSTER_NAME.yaml --state $KOPS_STATE_STORE'
                sh 'kops create secret --name value-source-cloud.com sshpublickey admin -i ~/.ssh/id_rsa.pub'
                sh 'kops update cluster $CLUSTER_NAME --state $KOPS_STATE_STORE --yes'
                */
                
                ///Update:
                /*
                sh 'kops create secret --name value-source-cloud.com sshpublickey admin -i ~/.ssh/id_rsa.pub'
                sh 'kops replace -f $CLUSTER_NAME.yaml'
                sh 'kops update cluster $CLUSTER_NAME --state $KOPS_STATE_STORE --yes'
                sh 'kops rolling-update cluster $CLUSTER_NAME --yes'
                */

               

                // This is the statement that created the cluster.
                sh 'kops create cluster $CLUSTER_NAME --zones us-east-2a --node-count 1 --node-size m4.large --master-size m4.large --dry-run -o yaml > $CLUSTER_NAME.yaml'
                //*********************************************************************



            }
            
    }

    stage('Deploy Pods') {
            steps {
                sh 'echo Deploy Pods'
                
                // sh 'kubectl apply -f dotnet-core-simple.yml'
               
                // sh 'kubectl run my-nginx --image=nginx --replicas=1 --port=80'
                
                // sh 'kubectl expose deployment my-nginx --port=80 --type=LoadBalancer'
                
                // sh 'kubectl get service -o wide'
                
            }
            
    }    
  }
}