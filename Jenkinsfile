pipeline {
    agent any
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
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
                branch '*/feature/*'
            }
            
            steps {
                 echo 'deploying to feature'
            }
        }
        stage('deploy-develop') {
            when {
                branch 'develop'
            }
            
            steps {
                 echo 'deploying to develop'
            }
        }
        stage('deploy-pre') {
            when {
                branch '*/release/*'
            }
            
            steps {
                 echo 'deploying to pre'
            }
        }
        stage('deploy-int') {
            when {
                branch '*/release/*'
            }
            
            steps {
        		input "want to deploy on int"
                echo 'deploying to INT'
            }
        }
        stage('deploy-PROD') {
            when {
                branch '*/release/*'
            }
            
            steps {            	
        		input "want to deploy on PROD"
                echo 'deploying to Prod'
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