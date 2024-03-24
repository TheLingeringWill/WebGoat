pipeline {
    agent any
    tools{
        maven 'Maven'
        jdk 'JDK17'
    }
    stages{
        stage('Clone repo'){
            steps{
                git branch: 'main', url: 'https://github.com/TheLingeringWill/WebGoat'
            }
        }
        stage('Apply Spotless'){
            steps{
                sh 'mvn spotless:apply'
            }
        }
        stage('Build'){
            steps{
                sh 'java -version'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Dependency checker'){
            steps{
                sh 'mvn org.owasp:dependency-check-maven:check -Dformat=ALL'
                dependencyCheckPublisher pattern: '**/target/dependency-check-report.xml'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('sonarqube'){
                    sh 'mvn clean verify sonar:sonar -Dmaven.test.skip=true -Dmaven.test.failure.ignore=true -Dsonar.projectName=webgoat -Dsonar.projectKey=webgoat -Dsonar.propjectVersion=1.0 -Dsonar.exclusions=**/*.ts -Dmaven.login=sqp_ecce68174c944a2e2a796368a0532eef253f5212'
                }
            }
        }
    }
}