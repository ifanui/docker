pipeline { 
    agent { node { label 'coreos' } }
    parameters {
        string(name: 'repository_url', defaultValue: 'git@github.com:ifanui/docker.git', description: 'Git hub repository url')
	string(name: 'docker.hub', defaultValue: 'ifanui/it-academy', description: 'Docker hub repository url')
        booleanParam(name: 'cheack_status', defaultValue: true, description: 'Check status web service') 
	booleanParam(name: 'clean_images', defaultValue: false, description: 'Delete all images after deploy coreos on coreos')
        booleanParam(name: 'clean_containers', defaultValue: true, description: 'Delete all containerss after deploy coreos on coreos')
    }
    stages {
	stage (' Clone repository'){
	     steps {
                   git url "${params.repository_url}"
		}
	}
        stage('Docker pull $params.docker.hub') { 
            steps { 
                   sh ' docker pull "${params.docker.hub}"'
            }
        }
	stage('Docker run') {
	    steps {
		sh 'docker run   -p 8400:8400 -p 8500:8500   -p 8600:53/udp   -v $WORKSPACE/docker/consul/config:/consul/config -d  ifanui/it-academy:latest'
		}
        }
	stage ('Check web service status on consul') {
	    when {
                expression {params.check_status == true}
            }   
            steps {
                sh '''
		curl http://127.0.0.1:8500/v1/health/service/web
                '''
            }   
        }   

	stage('Clean containers from deploy') {
            when {
                expression {params.clean_containers == true}
            }
            steps {
                sh '''
                docker rm --force $(docker ps | grep nginx | tr -s ' ' | cut -d ' ' -f 1)
                '''
            }
        }
        stage('Clean images from deploy') {
            when {
                expression {params.clean_images == true}
            }
            steps {
                sh '''
                docker rmi $(docker images | grep nginx | tr -s ' ' | cut -d ' ' -f 3)
                '''
            }
        }
}
	post {
            success {
                slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            }
            failure {
                slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            }
        }
}


}
