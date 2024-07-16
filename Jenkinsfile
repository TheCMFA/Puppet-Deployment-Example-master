pipeline {
    agent any
    environment {

        PUPPET_MASTER_URL  = '35.184.65.50'
        PUPPET_AGENT_URL_DEV = "35.222.16.182"
        PUPPET_AGENT_URL_PROD = "34.122.242.179"

        PUPPET_MASTER_HOME = '/home/jenkins'
        PUPPET_AGENT_HOME = '/home/jenkins'

        PUPPET_MASTER_MANIFEST_DIR = '/etc/puppet/code/environments/production/manifests'
        PUPPET_MASTER_MODULE_MANIFEST_DIR = '/etc/puppet/code/environments/production/modules/mymodule/manifests'
        PUPPET_MASTER_DEV_FILES_DIR = '/etc/puppet/code/environments/production/modules/mymodule/files'
        PUPPET_MASTER_PROD_FILES_DIR = '/etc/puppet/code/environments/production/modules/mymodule/files'

    }
    stages {
        stage("BuildTests") {
            when {
                branch 'develop'
            }
            steps {
                echo 'Building tests...'
                sh '''
                    # Comandos de construcción de tests sin Docker
                '''
            }
        }
        stage("RunTests") {
            when {
                branch 'develop'
            }
            steps {
                echo 'Running tests...'
                sh '''
                    # Comandos para ejecutar tests sin Docker
                '''
            }
        }
        stage("Build") {
            when {
                branch 'develop'
            }
            steps {
                echo 'Building application for deployment...'
                sh '''
                    # Comandos de construcción de la aplicación sin Docker
                '''
            }
        }
        stage("PushBuilds") {
            when {
                branch 'develop'
            }
            steps {
                echo "Pushing builds..."
                sh '''
                    # Comandos para subir builds sin Docker
                '''
            }
        }
        stage("DeployDev") {
            when {
                branch 'develop'
            }
            steps {
                echo "Deploying to development..."
                sh '''

                    echo "New deployment" >> deployments.txt
                    scp deployments.txt jenkins@${PUPPET_AGENT_URL_DEV}:${PUPPET_AGENT_HOME}/

                    scp site.pp jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/
                    scp init.pp jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/

                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/site.pp ${PUPPET_MASTER_MANIFEST_DIR}/
                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/init.pp ${PUPPET_MASTER_MODULE_MANIFEST_DIR}/

                '''
            }
        }
        stage("DeployProd") {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to production...'
                sh '''

                    echo "New deployment" >> deployments.txt
                    scp deployments.txt jenkins@${PUPPET_AGENT_URL_PROD}:${PUPPET_AGENT_HOME}/

                    scp site.pp jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/
                    scp init.pp jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/

                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/site.pp ${PUPPET_MASTER_MANIFEST_DIR}/
                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/init.pp ${PUPPET_MASTER_MODULE_MANIFEST_DIR}/

                ''' 
            }
        }
    }
}
