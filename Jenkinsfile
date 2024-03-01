pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonarsonar.host.url=http://localhost:9000 -Dsonarsonar.login=42632a7ae0f49d633e268018706f64cb1912f4cd
-Dsonarsonar.java.binaries=target
-Dsonarsonar.qualitygate.wait=true
-Dsonarsonar.java.test.sources=src/test
-Dsonarsonar.java.sources=src/main
-Dsonarsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java
-Dsonarsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
-Dsonarsonar.java.libraries=C:/Program Files/Maven/lib
-Dsonarsonar.java.test.libraries=C:/Program Files/Maven/lib"
            }
        }
    }
}