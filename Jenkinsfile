pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jacksonyap43124/Site'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh 'docker exec ansible ansible-playbook playbooks/edit-store.yml -i inventory/hosts.ini'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed — check the logs above'
        }
    }
}
