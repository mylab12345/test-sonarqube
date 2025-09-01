pipeline {                                    // 1  // Defines the start of the Jenkins pipeline block

    agent any                                 // Specifies the pipeline can run on any available agent

    environment {                             // 2  // Defines environment variables for the pipeline
        PATH = "/opt/maven/bin:$PATH"         // Adds Maven's path to the system's PATH variable
        SONAR_PROJECT_KEY = 'keshav-b01_keshav92'  // SonarCloud project key
        SONAR_ORGANIZATION = 'keshav'             // SonarCloud organization
        SONAR_TOKEN = credentials('sonarcloud-token') // Jenkins credential ID for SonarCloud token
    }                                         // 2  // Ends the environment block

    stages {                                  // 3  // Defines the stages block where multiple stages are declared
        
        stage("build") {                      // 4  // Creates a stage named 'build'
            steps {                           // 5  // Defines the steps that will be executed in this stage
                echo "----------- build started ----------"  
                sh 'mvn clean deploy -Dmaven.test.skip=true'  
                echo "----------- build completed ----------"  
            }                                 // 5  // Ends the steps block for 'build' stage
        }                                     // 4  // Ends the 'build' stage

        stage("test") {                       // 6  // Creates a stage named 'test'
            steps {                           // 7  // Defines the steps that will be executed in this stage
                echo "----------- unit test started ----------"  
                sh 'mvn surefire-report:report'  
                echo "----------- unit test completed ----------"  
            }                                 // 7  // Ends the steps block for 'test' stage
        }                                     // 6  // Ends the 'test' stage

        stage('SonarQube analysis') {         // 8  // Creates a stage named 'SonarQube analysis'
            steps {                           // 9  // Defines the steps that will be executed in this stage
                withSonarQubeEnv('keshav-sonarqube-server') {
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.organization=${SONAR_ORGANIZATION} \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=${SONAR_TOKEN}
                    '''
                }                             // Ends the withSonarQubeEnv block
            }                                 // 9  // Ends the steps block for 'SonarQube analysis' stage
        }                                     // 8  // Ends the 'SonarQube analysis' stage

    }                                         // 3  // Ends the stages block
}                                             // 1  // Ends the pipeline block
