serverId: 'arty'
   pipeline {
	   agent any
     tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages{
        stage ('build') {
            steps {
        echo "code is building"
         sh 'mvn clean'
         sh 'mvn install'
            }
        }
	    
	stage('Build Docker Image') {
           steps {
               echo 'Building Docker image'
               sh 'docker build -t pro .'
                }
            }
        stage ('Uploading to Ecr') {
            steps {
                echo "uploading to ECR "
                sh '$(aws ecr get-login --no-include-email --region ap-south-1)'
                sh 'docker tag pro:latest 937382548142.dkr.ecr.ap-south-1.amazonaws.com/pro:latest'
                sh 'docker push 937382548142.dkr.ecr.ap-south-1.amazonaws.com/pro:latest'
            }
        }
        
        
        stage ('Deploying to eks') {
            steps {
                 echo "Deploying imgae to EKS"
                 sh 'rm -rf /var/lib/jenkins/.kube/ && aws eks update-kubeconfig --name myeks1 --region ap-south-1'
                 sh 'kubectl apply -f pro.yml'
                 sh 'kubectl apply -f prosvc.yml'
            }
        }
}
}
