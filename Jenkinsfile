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
                echo pwd()
                sh 'scp -o StrictHostKeyChecking=no target/DevOpsRocks.war root@3.135.193.143:/root/tomcat/webapps/DevOpsRocks.war'
            }
        }	
		
    }
}
