pipeline {
    agent any

    environment {
        DOTNET_ROOT = tool name: 'dotnet-sdk', type: 'Dotnet'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/StudentDetailsApp.git'
            }
        }
        stage('Restore') {
            steps {
                sh '${DOTNET_ROOT}/dotnet restore'
            }
        }
        stage('Build') {
            steps {
                sh '${DOTNET_ROOT}/dotnet build --configuration Release'
            }
        }
        stage('Test') {
            steps {
                sh '${DOTNET_ROOT}/dotnet test'
            }
        }
        stage('Publish') {
            steps {
                sh '${DOTNET_ROOT}/dotnet publish --configuration Release --output ./publish'
            }
        }
        stage('Deploy to Azure') {
            steps {
                script {
                    def webAppName = 'your-webapp-name'
                    def resourceGroupName = 'your-resource-group'
                    def appServicePlan = 'your-app-service-plan'

                    sh """
                    az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID
                    az webapp create --name $webAppName --resource-group $resourceGroupName --plan $appServicePlan
                    az webapp config appsettings set --name $webAppName --resource-group $resourceGroupName --settings WEBSITES_PORT=80
                    az webapp deployment source config-zip --resource-group $resourceGroupName --name $webAppName --src ./publish.zip
                    """
                }
            }
        }
    }
}
