pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Runs every 3 minutes on Mondays
    }

    environment {
        MAVEN_HOME = "/opt/homebrew/opt/maven"
        PATH = "$MAVEN_HOME/bin:$PATH"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/nahom96/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh '$MAVEN_HOME/bin/mvn clean install'
            }
        }

        stage('Run Tests & Code Coverage') {
            steps {
                sh '$MAVEN_HOME/bin/mvn test'
            }
        }

        stage('Generate Jacoco Report') {
            steps {
                sh '$MAVEN_HOME/bin/mvn jacoco:report'
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
