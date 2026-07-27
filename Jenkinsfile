pipeline {
    agent any

    stages {
        stage('git scm update') {
            steps {
                git url: 'https://github.com/wkfkeha784-pixel/ktcloudinfrajenkins.git',
                    branch: 'main'
            }
        }

        stage('docker image build') {
            steps {
                sh '''
                    docker build -t hcpark97/ktcloudinfra4:0727 .
                '''
            }
        }

        stage('push to docker hub') {
            steps {
                sh '''
                    docker push hcpark97/ktcloudinfra4:0727
                '''
            }
        }

        stage('copy deploy.yml to master') {
            steps {
                sh '''
                    ansible master -m copy \
                    -a "src=${WORKSPACE}/deploy.yml dest=/root/deploy.yml mode=0644"
                '''
            }
        }

        stage('deploy to kubernetes') {
            steps {
                sh '''
                    ansible master -m shell \
                    -a "kubectl --kubeconfig=/etc/kubernetes/admin.conf apply -f /root/deploy.yml"
                '''
            }
        }

        stage('verify deployment') {
            steps {
                sh '''
                    ansible master -m shell \
                    -a "kubectl --kubeconfig=/etc/kubernetes/admin.conf get deploy,pod,svc -o wide"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
