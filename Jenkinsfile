pipeline
{
    agent any
    stages
    {
        stage ('Download')
        {
            steps
            {
                git 'https://github.com/nareshvtvc/DevOps-Project.git'
            }
        }
        
        stage ('Build')
        {
            steps
            {
                sh label: '', script: 'mvn package'
            }
        }
	stage ('Junit Test Reports')
        {
            steps
            {
                junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
            }
        }
		
		stage ('Deploy')
        {
            steps
            {
                 sh 'cp target/DevOpsRocks.war root@3.135.193.143:/opt/tomcat/webapps/DevOpsRocks.war'
            }
        }	
		
    }
}
