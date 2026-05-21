pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-token')
    }

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

        stage('Push Changes to GitHub') {
            steps {
                sh '''
                    docker cp ansible:/ansible/playbooks/index.html index.html
                    git config user.email "jenkins@local.com"
                    git config user.name "Jenkins"
                    git add index.html
                    git diff --staged --quiet || git commit -m "Auto update by Ansible via Jenkins"
                    git push https://$GITHUB_TOKEN@github.com/jacksonyap43124/Site main
                '''
            }
        }
    }

    post {
        success { echo 'Done! Site updated and pushed to GitHub!' }
        failure { echo 'Something went wrong — check logs above' }
    }
}
