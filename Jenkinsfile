pipeline {
    agent any 
    stages {
        stage('Clone the repo') {
            steps {
                echo 'clone the repo'
                sh 'rm -fr gcp-demo'
                sh 'git clone https://github.com/sujithkuchipudi268/gcp-demo.git'
                sh 'pwd'
            }
        }
        stage('install app dependencies') {
            steps {
                sshagent(credentials : ['DeployServer']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no saivenkatasujith@34.125.43.56 hostname 
                    '''
                }
            }
        }
        stage('install app2 dependencies') {
            steps {
                sshagent(credentials : ['DeployServer']) {
                    sh '''
                    echo 'connect to remote host and install the app dependencies'
                    ssh -o StrictHostKeyChecking=no saivenkatasujith@34.125.43.56 sudo apt-get update -y
                    ssh -o StrictHostKeyChecking=no saivenkatasujith@34.125.43.56 sudo apt-get install apache2 -y
                    ssh -o StrictHostKeyChecking=no saivenkatasujith@34.125.43.56 sudo service apache2 start
                    '''
                  }
            }
        }
        stage('push repo to remote host') {
            steps {
                sshagent(credentials : ['DeployServer']) {
                    sh '''
                    echo 'connect to remote host and pull down the latest version'
                    ssh -o StrictHostKeyChecking=no saivenkatasujith@34.125.43.56 sudo chown -R saivenkatasujith.saivenkatasujith /var/www/html/
                    scp -r -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/sample/gcp-demo/html saivenkatasujith@34.125.43.56:/var/www/html/
                    '''
                }           
            }
        }
        stage('Check website is up') {
            steps {
                echo 'Check website is up'
                sh 'curl -Is 34.125.43.56 | head -n 1'
            }
        }
    }
}
