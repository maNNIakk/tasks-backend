pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit Tests') {
            steps {
                bat 'mvn verify'
            }
        }
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat script: """
                        "${scannerHome}/bin/sonar-scanner" ^
                        -Dsonar.projectKey=DeployBack ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=42632a7ae0f49d633e268018706f64cb1912f4cd ^
                        -Dsonar.java.binaries=target ^
                        -Dsonar.qualitygate.wait=true ^
                        -Dsonar.java.test.sources=src/test ^
                        -Dsonar.java.sources=src/main ^
                        -Dsonar.coverage.exclusions="**/.mvn/**,**/src/test/**,**/model/**,**Application.java" ^
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml ^
                        -Dsonar.java.libraries="C:/Program Files/Maven/lib" 
                        -Dsonar.java.test.libraries="C:/Program Files/Maven/lib"
                    """
                }
            }
        }
        stage('Deploy Backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        
        stage('API Test'){
            steps{
                dir('api-test'){
                git 'https://github.com/maNNIakk/tasks-api-test'
                bat 'mvn test'
                }
            }
        }
        
        stage('Deploy FrontEnd'){
            steps{
                dir('frontend'){
                git 'https://github.com/maNNIakk/tasks-frontend'
                bat 'mvn clean package'
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'

                }
            }
        }
        
         stage('Functional Test'){
            steps{
                dir('functional-test'){
                git 'https://github.com/maNNIakk/tasks-functional-test'
                bat 'mvn test'
                }
            }
        }
        stage('Deploy Project'){
            steps{
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }
    }
}
