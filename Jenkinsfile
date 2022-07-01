pipeline {
    agent any
    stages {
        stage('Git') {
            steps {
              git 'https://github.com/momerjavaidSystemsltd/mavenjava.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t omerj121/my-maven-docker-project.jar .'
            }
        }
        stage('Run a container') {
            steps {
                  sh 'docker run -d -p 6060:8080 omerj121/my-maven-docker-project.jar'
            }
        }
        stage('Sonar Analyzer') {
            steps {
                     withSonarQubeEnv('sonarqube') {
                        sh "mvn clean verify sonar:sonar" 
                        sh "-Dsonar.projectKey=maven" 
                        sh "-Dsonar.host.url=http://20.54.72.51:9000"
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}

    
        
    
