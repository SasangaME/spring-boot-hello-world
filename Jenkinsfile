pipeline {
    agent any

    tools {
        maven 'M398'
    }

    stages {
        stage('Clean old jar') {
            steps {
                sh 'rm -f target/spring-boot-2-hello-world-*-SNAPSHOT.jar || true'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Run App') {
            steps {
                sh '''
                  JAR=$(ls target/spring-boot-2-hello-world-*-SNAPSHOT.jar | head -n 1)
                  echo "Running $JAR on 8081"

                  export JENKINS_NODE_COOKIE=dontKillMe

                  nohup java -jar "$JAR" --server.port=8081 > app.log 2>&1 &
                '''
            }
        }
    }
}
