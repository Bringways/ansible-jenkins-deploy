
pipeline {
    agent any

    stages {
        stage('GitHub Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR-GITHUB-USERNAME/ansible-jenkins-deploy.git'
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
            echo 'SUCCESS: Deployment completed on the target servers.'
        }
        failure {
            echo 'FAILED: Check Console Output for the error.'
        }
    }
}
