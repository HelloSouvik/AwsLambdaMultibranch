pipeline {
    agent any
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
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
                echo "PATH = ${PATH}"
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