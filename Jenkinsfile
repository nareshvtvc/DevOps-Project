#!groovy

pipeline
{
    agent any
	environment{
	DOCKER_TAG = getDockerTag()
	}
    stages
    {
        stage ('Download')
        {
            steps
            {
                git 'https://github.com/snteja/DevOps-Project.git'
            }
        }
        
        stage ('Build')
        {
            steps
            {
                sh 'mvn package'
            }
        }
		
	stage ('Build Dockerfile')
        {
            steps
            {
		sh "docker build . -t sainava225/snteja-app:${DOCKER_TAG}"
            }
        }
		
	stage ('Docker Push')
        {
            steps
            {
		withCredentials([string(credentialsId: 'dockerhub-teja', variable: 'dockerhubpwd')]) {
		sh "docker login -u sainava225 -p ${dockerhubpwd}"
			}
		echo "sainava225/snteja-app:${DOCKER_TAG}"
		sh "docker push sainava225/snteja-app:${DOCKER_TAG}"
			}	
        }
        
        stage ('Deploy to K8s')
        {
            steps
            {
                sh 'chmod +x  changeTag.sh'
				sh "./changeTag.sh ${DOCKER_TAG}"
				sshagent(['k8-ssh']) {
                    sh 'scp -o StrictHostKeyChecking=no teja-service.yml teja-pod.yml ubuntu@18.221.94.79:/home/ubuntu/'
		    script{
			try{
				sh 'ssh ubuntu@18.221.94.79 kubectl apply -f .'
				}catch(error){
				sh 'ssh ubuntu@18.221.94.79 kubectl create -f .'
				}
		    }
		}
            }
            }
        }
    }


def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
	return tag
}
