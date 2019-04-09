pipeline { 
    agent { node { label 'slave' } }
    stages ('update for pmp') {
        stage ('download pmp patch') {
            steps {
                sh "wget -O pmp-${BUILD_ID}.ppm http://archives.manageengine.com/passwordmanagerpro/9600/ManageEngine_PasswordManager_Pro_9500_9501_9502_to_9600.ppm"
            }
        }
        stage ('stop pmp service') {
            steps {
                sh "sudo /etc/init.d/pmp-service stop"
            }
        }
        stage ('update') {
            steps {
                dir ('/home/ubuntu/ManageEngine/PMP/bin/') {
                sh "sudo sh UpdateManager.sh -c -option i -ppmPath $WORKSPACE/pmp-${BUILD_ID}.ppm"
            }}
        }
        stage ('start pmp service') {
            steps {
                sh "sudo /etc/init.d/pmp-service start"
            }
        }
    }
}

