pipeline { 
    agent { node { label 'coreos' } }
    parameters {
        string(name: 'repository_url', defaultValue: 'git@github.com:ifanui/docker.git', description: 'Github repository url')
        booleanParam(name: 'clean_images', defaultValue: false, description: 'Delete all images after deploy nginx on coreos')
        booleanParam(name: 'clean_containers', defaultValue: true, description: 'Delete all containerss after deploy nginx on coreos')
    }
    stages {
        stage('Clone repository') { 
            steps { 
                    git  url: "${params.repository_url}"
            }
        }
	stage('Git push for repo') {
	    steps {
		sh 'cd  $WORKSPACE/docker | date > date.txt | git add date.txt | git commit -m "update repo $BUILD_ID" | git push -u origin master'
		}
        }
}
}
