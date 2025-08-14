pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'  // Skip host key checking
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Pulling latest code from GitHub..."
                git branch: 'master', url: 'https://github.com/Amit-4535/newforproject.git'
            }
        }

        stage('Run Ansible Playbook - Install Apache2') {
            steps {
                echo "Running Ansible Playbook to install & restart Apache2..."
                sh '''
                    ansible-playbook -i /etc/ansible/hosts apache2.yml
                '''
            }
        }

        stage('Deploy All Files to Nodes') {
            steps {
                echo "Deploying all files from GitHub repo to /var/www/html on node1 and node2..."
                sh '''
                    ansible all -i /etc/ansible/hosts -m copy \
                        -a "src=. dest=/var/www/html owner=www-data group=www-data mode=0644 recurse=yes"
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully! Open browser on node1/node2 IP to check."
        }
        failure {
            echo "Deployment failed. Please check the logs."
        }
    }
}

