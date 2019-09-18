pipeline {
    agent none
    environment {
        CI = 'true' 
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:6-alpine' 
                    args '-p 3001:3000' 
                }
            }
            steps {
                sh 'npm install'
                sh 'npm run-script build'
            }
        }
        stage('Test') { 
            agent {
                docker {
                    image 'node:6-alpine' 
                    args '-p 3001:3000' 
                }
            }
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Aprovisionar') {
            agent {
                docker { 
                    image 'mullnerz/ansible-playbook'
                }
            }
            steps {
                sh './infraestructura/provision.sh ./infraestructura/inventory.demo1'
            }
        }

        stage('Desplegar') {
            agent {
                docker { 
                    image 'mullnerz/ansible-playbook'
                }
            }
            steps {
                sh './infraestructura/provision.sh ./infraestructura/inventory.demo1.deploy'
            }
        }
    }
}
