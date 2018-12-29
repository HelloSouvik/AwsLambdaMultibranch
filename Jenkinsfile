pipeline {
    agent any
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
    }
    tools { 
        maven 'Maven 3.5.3' 
        jdk 'jdk8' 
    }
    stages {
        stage('Build') {
            steps {
                echo '***********************************************'
                echo '*************** Env Var ************************'
                echo '***********************************************'
                echo "env.PATH = ${env.PATH}"
                echo "env.BRANCH_NAME = ${env.BRANCH_NAME}"
                echo "env.CHANGE_ID = ${env.CHANGE_ID}"
                echo "env.CHANGE_URL = ${env.CHANGE_URL}"
                echo "env.CHANGE_TITLE = ${env.CHANGE_TITLE}"
                echo "env.CHANGE_AUTHOR = ${env.CHANGE_AUTHOR}"
                echo "env.CHANGE_AUTHOR_DISPLAY_NAME = ${env.CHANGE_AUTHOR_DISPLAY_NAME}"
                echo "env.CHANGE_AUTHOR_EMAIL = ${env.CHANGE_AUTHOR_EMAIL}"
                echo "env.BUILD_NUMBER = ${env.BUILD_NUMBER}"
                echo "env.BUILD_ID = ${env.BUILD_ID}"
                echo "env.BUILD_DISPLAY_NAME = ${env.BUILD_DISPLAY_NAME}"
                echo "env.JOB_NAME = ${env.JOB_NAME}"
                echo "env.JOB_BASE_NAME = ${env.JOB_BASE_NAME}"
                echo "env.BUILD_TAG = ${env.BUILD_TAG}"
                echo "env.EXECUTOR_NUMBER = ${env.EXECUTOR_NUMBER}"
                echo "env.NODE_NAME = ${env.NODE_NAME}"
                echo "env.NODE_LABELS = ${env.NODE_LABELS}"
                echo "env.WORKSPACE = ${env.WORKSPACE}"
                echo "env.JENKINS_HOME = ${env.JENKINS_HOME}"
                echo "env.JENKINS_URL = ${env.JENKINS_URL}"
                echo "env.BUILD_URL = ${env.BUILD_URL}"
                echo "PATH = ${PATH}"
                echo "AWS_ACCESS_KEY_ID = ${AWS_ACCESS_KEY_ID}"
                echo "AWS_SECRET_ACCESS_KEY = ${AWS_SECRET_ACCESS_KEY}"
                
                bat 'mvn clean install -Dmaven.test.failure.ignore=true' 
            }
            post {
                success {
                    echo 'success Building bat set done-1'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deliver for feature') {
            when {
                branch '*/feature/*'
            }
            
            steps {
                 bat 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
        }
        
        stage('Deliver for development') {
            when {
                branch 'develop'
            }
            
            steps {
                 bat 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Deploy for pre') {
            when {
                branch '*/release/*'
            }
            steps {
                bat 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
		}
		stage('Sanity check') {
            steps {
                input "want to deploy on production"
            }
        }
        stage('Deploy - Production') {
        	when {
                branch '*/release/*'
            }
   			steps {
        		echo 'Production - Deploying'
    		}
		}
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}