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
                bat 'mvn test'
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
                        -Dsonar.coverage.exclusions='**/.mvn/**,**/src/test/**,**/model/**,**Application.java' ^
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml ^
                        -Dsonar.java.libraries="C:/Program Files/Maven/lib" 
                        -Dsonar.java.test.libraries="C:/Program Files/Maven/lib"
                    """
                }
            }
        }
    }
}
