pipeline {
    agent any
    tools{
        jdk 'jdk11'
        maven 'maven3'
    }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('git-checkout') {
            steps {
                git 'https://github.com/raj-git-89/secretsanta-generator.git'
            }
        }

        stage('Code Compile') {
            steps {
               sh "mvn clean compile"
            }
        }
        
        stage('Unit Tests') {
            steps {
               sh "mvn test"
            }
        }


        stage('Sonar Analysis') {
            steps {
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://34.16.187.169:9000/ -Dsonar.login=squ_422c6057e451eb81e7c78a098ddf3f234a629f48 -Dsonar.projectName=Santa \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Santa '''
            }
        }
        		 
        stage('Code-Build') {
            steps {
               sh "mvn clean install"
            }
        }

         stage('Docker Build') {
            steps {
               script{
                   withDockerRegistry(credentialsId: '854e6db5-573c-41fb-bd0a-e50e9517e02b', toolName: 'docker') {
                    sh "docker build -t  santa . "
                 }
               }
            }
        }

        stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: '854e6db5-573c-41fb-bd0a-e50e9517e02b', toolName: 'docker') {
                    sh "docker tag santa rajrajendraray89/santa:latest"
                    sh "docker push rajrajendraray89/santa:latest"
                 }
               }
            }
        }
        
        stage('Docker Deploy') {
            steps {
               script{
                   withDockerRegistry(credentialsId: '854e6db5-573c-41fb-bd0a-e50e9517e02b', toolName: 'docker') {
                    sh "docker run -d --name santa123 -p 9090:8080 rajrajendraray89/santa:latest"
                 }
               }
            }
        }
    }    
}
