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

		stage ('nuget restore')
		{
			steps
			{				
				bat "dotnet restore"
			}
		}

		// stage ('Start sonarqube analysis')
		// {	
		// 	steps
		// 	{				
		// 		withSonarQubeEnv('Test_Sonar')
		// 		{
		// 			bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll begin /k:sonar-rajeevray"  
		// 		}                
		// 	}
		// }
		
		// stage ('clean and build')
		// {
		// 	steps
		// 	{				
        //         bat "dotnet clean"
								
		// 		bat "dotnet build -c Release"
		// 	}	
		// }

		// stage ('Test case execution')
		// {
		// 	steps
		// 	{				
        //        bat "dotnet test test-project\\test-project.csproj -l:trx;LogFileName=nagp-us-devops-test-output.xml"
		// 	}	
		// }

		// stage ('SonarQube Analysis end')
		// {				
		// 	steps
		// 	{				
		// 		withSonarQubeEnv('Test_Sonar')
		// 		{
		// 			bat """
		// 			dotnet "${scannerHome}\\SonarScanner.MSBuild.dll" end
		// 			"""
		// 		}
		// 	}
		// }

		stage ('Kubernetes Deployment')
		{				
			steps
			{				
				//bat "kubectl apply -f deployment.yaml"

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
