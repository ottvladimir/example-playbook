pipeline {
    agent any

    stages {
        stage('CLONE_GIT_REPO') {
            steps {
                sh 'git clone https://github.com/ottvladimir/example-playbook.git'
                }
        }
        stage('ADD_SSH_SETTINGS') {
            steps {
                sh 'mkdir ~/.ssh'
                sh 'ssh-keyscan -t rsa github.com | tee github-key-temp | ssh-keygen -lf -'
                sh 'cat github-key-temp >> ~/.ssh/known_hosts'
                }
        }
        stage('DECRYPT_VAULT') {
            steps {
                dir("example-playbook"){
                    sh 'ansible-vault decrypt secret --vault-password-file vault_pass'
                    sh 'mv ./secret ~/.ssh/id_rsa'
                    sh 'chmod 400 ~/.ssh/id_rsa'
                }
            }
        }
        stage('RUN_ANSIBLE') {
            steps {
                dir("example-playbook"){
                    sh 'ansible-galaxy install -r requirements.yml -p roles'
                    sh 'ansible-playbook site.yml -i inventory/prod.yml'
                }
            }
        }
        stage('RUN_SOMETHING') {
            steps {
                sh 'ls -lah example-playbook/roles'
            }
        }    
    }
}
