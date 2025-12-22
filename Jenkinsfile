pipeline {
    agent any

    tools {
        maven 'M398'   // Jenkins-managed Maven
    }


    // environment {
    //     JAVA_HOME = "/usr/lib/jvm/java-21-openjdk-amd64"
    //     M398     = "/opt/apache-maven-3.9.8"
    //     PATH     = "${env.JAVA_HOME}/bin:${env.M398}/bin:${env.PATH}"
    // }

    stages {
        // stage('Checkout') {
        //     steps {
        //         git branch: 'main',
        //             url: 'https://github.com/your-org/your-spring-boot-app.git'
        //     }
        // }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            // post {
            //     always {
            //         junit 'target/surefire-reports/*.xml'
            //     }
            // }
        }

        stage('Run App') {
            when {
                expression { return env.BRANCH_NAME == "main" }
            }
            steps {
                sh '''
                  JAR_FILE=$(ls target/*.jar | head -n 1)
                  echo "Running $JAR_FILE"
                  nohup java -jar "$JAR_FILE" --server.port=8081 > app.log 2>&1 &
                '''
            }
        }
    }

    // post {
    //     always {
    //         archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    //     }
    // }
}
