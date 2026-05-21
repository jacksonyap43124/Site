pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jacksonyap43124/Site'
            }
        }

        stage('Copy HTML to Ansible') {
            steps {
                sh 'docker cp index.html ansible:/ansible/playbooks/index.html'
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
            echo 'Done! Site updated successfully!'
        }
        failure {
            echo 'Something went wrong — check logs above'
        }
    }
}
