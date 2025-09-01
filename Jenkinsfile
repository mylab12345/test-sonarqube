pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
        scannerHome = tool 'keshav-sonar-scanner'
    }

    stages {
        stage("build") {
            steps {
                echo "----------- build started ----------"
                sh 'mvn clean deploy -DskipTests'
                echo "----------- build completed ----------"
            }
        }

        stage("test") {
            steps {
                echo "----------- unit test started ----------"
                sh 'mvn test jacoco:report'
                echo "----------- unit test completed ----------"
            }
        }

        stage("SonarQube analysis") {
            steps {
                withSonarQubeEnv('keshav-sonarqube-server') {
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                              -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }
}
