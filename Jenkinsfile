pipeline {

    agent {
        node {
            label 'JENKINS-AGENT'
        }
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        stage('Printing info') {
            steps {
            echo 'Branch name and revision:'
            sh '''
                git name-rev --name-only HEAD
                git rev-list --parents -n 1 HEAD
            '''

            echo 'Multibeanch pipeline env:'
            sh '''
                echo "BRANCH_NAME = ${BRANCH_NAME}"
                echo "CHANGE_ID = ${CHANGE_ID}"
                echo "CHANGE_BRANCH = ${CHANGE_BRANCH}"
                echo "CHANGE_TARGET = ${CHANGE_TARGET}"
            '''
            }
        }
 
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                ])
            }
        }

        stage('Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Deploy To Dev & QA') {
            when {
                branch 'develop'
            }
            steps {
                sh """
                echo "Building Artifact for Dev Environment"
                """
                sh """
                echo "Deploying to Dev Environment"
                """
                sh """
                echo "Deploying to QA Environment"
                """
            }
        }

        stage('Deploy To Staging & Pre-Prod') {
            when {
                branch 'master'
            }
            steps {
                sh """
                echo "Building Artifact for Staging and Pre-Prod Environments"
                """
                sh """
                echo "Deploying to Staging Environment"
                """
                sh """
                echo "Deploying to Pre-Prod Environment"
                """
            }
        }

    }   
}
