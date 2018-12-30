pipeline {
    agent any
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
        ENVIRONMENT_NAME = 'dev'
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
        string(name: 'AnoGreeting', defaultValue: 'AnoHello', description: 'AnoHow should I greet the world?')
    }
    tools { 
        maven 'Maven 3.5.3' 
        jdk 'jdk8' 
    }
    stages {
        stage('Build') {
            steps {                
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
        stage('deploy-feature') {
            when {
                branch '*feature/*'
            }
            
            steps {
            	ENVIRONMENT_NAME = 'feature'
                echo 'deploying to feature ${ENVIRONMENT_NAME}'
            }
        }
        stage('deploy-develop') {
            when {
                branch 'develop'
            }
            
            steps {            	
            	ENVIRONMENT_NAME = 'dev'
                echo 'deploying to develop ${ENVIRONMENT_NAME}'
            }
        }
        stage('preparing release branch') {
            when {
                branch '*release/*'
            }
            stages {
            	stage('deploy-pre') {
            		steps {
            			ENVIRONMENT_NAME = 'pre'
		                echo 'deploying to pre ${ENVIRONMENT_NAME}'
		            }
            	}
            	stage('deploy-int') {
            		steps {
            			input "want to deploy on int"
            			ENVIRONMENT_NAME = 'int'
		                echo 'deploying to int ${ENVIRONMENT_NAME}'
		            }
            	}
            	stage('deploy-prod') {
            		steps {
            			input "want to deploy on PROD"
            			ENVIRONMENT_NAME = 'prod'
		                echo 'deploying to prod ${ENVIRONMENT_NAME}'
		            }
            	}
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