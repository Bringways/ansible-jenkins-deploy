
pipeline {
    agent any

    stages {
        stage('GitHub Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Bringways/ansible-jenkins-deploy.git'
            }
        }

        stage('Check Files') {
            steps {
                sh '''
                    set -e
                    test -f inventory.ini
                    test -f deploy.yml
                    test -f index.html
                    echo "All required files are present."
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                ansiblePlaybook(
                    credentialsId: 'Ansible.pem',
                    installation: 'ansible',
                    inventory: 'inventory.ini',
                    playbook: 'deploy.yml',
                    disableHostKeyChecking: true
                )
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Deployment completed!'
        }
        failure {
            echo 'FAILED: Check Console Output.'
        }
    }
}
