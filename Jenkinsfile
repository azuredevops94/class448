pipeline {

agent any

environment {

ACR_NAME="devopsacr12345"
IMAGE_NAME="devops-app"
RESOURCE_GROUP="devops-rg"
AKS_CLUSTER="devops-aks"

}

stages {

stage('Checkout Code') {
steps {
git 'https://github.com/azuredevops94/class448'
}
}

stage('Install Dependencies') {
steps {
sh 'npm install'
}
}

stage('Run Unit Tests') {
steps {
sh 'npm test || true'
}
}

stage('Build Docker Image') {
steps {
sh '''
docker build -t $ACR_NAME.azurecr.io/$IMAGE_NAME:latest .
'''
}
}

stage('Push Image to Azure Container Registry') {
steps {
sh '''
az acr login --name $ACR_NAME
docker push $ACR_NAME.azurecr.io/$IMAGE_NAME:latest
'''
}
}

stage('Deploy to AKS') {
steps {
sh '''
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
'''
}
}

}

}
