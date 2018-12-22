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
                echo 'Building bat set done-1'
                echo "PATH = ${PATH}"
                bat 'mvn clean install -Dmaven.test.failure.ignore=true' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
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
                branch 'feature/*'
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
                branch 'release/*'
            }
            steps {
                bat 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
            steps {
                input "want to deploy on int"
            }
            steps {
                bat 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
            steps {
                input "want to deploy on prod"
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