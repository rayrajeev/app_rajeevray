pipeline
{
	agent any
	environment
	{
		scannerHome = tool name: 'sonar_scanner_dotnet'
		sonarjobName = "sonar-rajeevray"		     		
        userName = "rajeevray"

		//GKE Section
		PROJECT_ID = "groovy-environs-338318"
		CLUSTER_NAME= "nagp-devops-cluster"
		LOCATION= "us-east1-b"
		CREDENTIAL_ID= "GKE-Kubernetes"
	}
	options
	{			
		timestamps ()	
			
		timeout(time: 1, unit: 'HOURS')
			 			 
		// Discard old builds after 5 days or 5 builds count.
		buildDiscarder(logRotator(daysToKeepStr: '4', numToKeepStr: '4'))
			  
		disableConcurrentBuilds()
	}
		 
	stages
	{
        stage ('Checkout')	{
            steps
			{
                git branch: 'master', url: 'https://github.com/rayrajeev/app_rajeevray.git'
            }
        }

		stage ('Nuget restore')
		{
			steps
			{				
				bat "dotnet restore"
			}
		}

		stage ('Clean and build')
		{
			steps
			{				
                bat "dotnet clean"
								
				bat "dotnet build -c Release"
			}	
		}

		stage ('Release Artifacts')
		{		
			steps
			{				
				bat "dotnet publish nagp-devops-us -o nagp-devops-us/app/publish"
			}
		}		

		stage ('Kubernetes Deployment')
		{				
			steps
			{				
				step([
                $class: 'KubernetesEngineBuilder',
                projectId: env.PROJECT_ID,
                clusterName: env.CLUSTER_NAME,
                location: env.LOCATION,
                manifestPattern: 'deployment.yaml',
                credentialsId: env.CREDENTIAL_ID])
			}
		}			
	}	 
}
