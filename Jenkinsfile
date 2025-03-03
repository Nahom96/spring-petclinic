pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Runs every 3 minutes on Mondays
    }

    environment {
        MAVEN_HOME = tool 'Maven 3' // Ensure you have Maven configured in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/nahom96/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests & Code Coverage') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Generate Jacoco Report') {
            steps {
                sh 'mvn jacoco:report'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Publish Jacoco Report') {
            steps {
                jacoco execPattern: 'target/jacoco.exec'
            }
        }
    }
}
