pipeline {
    agent any 
    stages {
        stage('Clone the repo') {
            steps {
                echo 'clone the repo'
                sh 'rm -fr demo-ggcp'
                sh 'git clone https://github.com/ravvereddy/demo-ggcp.git'
                sh 'pwd'
            }
        }
        stage('install app dependencies') {
            steps {
                sshagent(credentials : ['DeployServer']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@13.232.207.109 hostname 
                    '''
                }
            }
        }
        stage('install app2 dependencies') {
            steps {
                sshagent(credentials : ['DeployServer']) {
                    sh '''
                    echo 'connect to remote host and install the app dependencies'
                    ssh -o StrictHostKeyChecking=no ubuntu@13.232.207.109 sudo apt-get update -y
                    ssh -o StrictHostKeyChecking=no ubuntu@13.232.207.109 sudo apt-get install apache2 -y
                    ssh -o StrictHostKeyChecking=no ubuntu@13.232.207.109 sudo service apache2 start
                    '''
                  }
            }
        }
        stage('push repo to remote host') {
            steps {
                sshagent(credentials : ['DeployServer']) {
                    sh '''
                    echo 'connect to remote host and pull down the latest version'
                    ssh -o StrictHostKeyChecking=no ubuntu@13.232.207.109 sudo chown -R ubuntu.ubuntu /var/www/html/
                    scp -r -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/sample/demo-ggcp/html ubuntu@13.232.207.109:/var/www/html/
                    '''
                }           
            }
        }
        stage('Check website is up') {
            steps {
                echo 'Check website is up'
                sh 'curl -Is 13.232.207.109 | head -n 1'
            }
        }
    }
}
