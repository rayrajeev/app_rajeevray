pipeline
{
	agent any
	environment
	{
		scannerHome = tool name: 'sonar_scanner_dotnet', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'
		jobName = "America's NAGP DevOps Assignment"
		dockerRegistry = "dtr.nagarro.com:443"       		
        userName = "rajeevray"
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
		
		stage ('clean and build')
		{
			steps
			{				
                bat "dotnet clean"
								
				bat "dotnet build -c Release"
			}	
		}			
	}	 
}
