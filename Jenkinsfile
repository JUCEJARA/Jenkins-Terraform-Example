pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    environment {
        TFSEC_DOCKER_CMD = 'docker'
        TERRAFORM_CMD = 'terraform'
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('tfsec') {
            steps {
                bat(script: '"${env.TFSEC_DOCKER_CMD}" run --rm -v "%WORKSPACE%:/src" aquasec/tfsec .')
            }
        }
        stage('Approval for Terraform') {
            steps {
                input(message: 'Approval required before Terraform', ok: 'Proceed', submitterParameter: 'APPROVER')
            }
        }
        stage('terraform') {
            steps {
                bat(script: '"${env.TERRAFORM_CMD}" apply -auto-approve -no-color')
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

