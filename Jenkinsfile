pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                cleanWs()
                checkout scm: [$class: 'GitSCM', branches: [
                    [name: '*/master']
                ], userRemoteConfigs: [
                    [credentialsId: 'github_token', url: 'https://github.com/ankitkumar-devops/spring-boot-tutorial.git']
                ]]
            }
        }
        stage('Build') {
            steps {
                // Build the project using Maven
                sh 'mvn clean install'
            }
        }
        stage('mvn test') {
            when {
                expression {
                    return params.ENVIRONMENT == 'TEST'
                }
            }
            steps {
                sh 'mvn test'
            }

        }
        stage('Deploy to Dev') {
            when {
                expression {
                    return params.ENVIRONMENT == 'DEV'
                }
            }
            steps {
                script {
                    // Copy the JAR to the remote server using SCP
                    sh 'scp target/spring-boot-tutorial-0.0.1-SNAPSHOT.jar ubuntu@54.234.80.65:/home/ubuntu'

                    // SSH into the server and kill the current process
                    sh 'ssh -tt ubuntu@54.234.80.65 "sudo pkill -f spring-boot-tutorial-0.0.1-SNAPSHOT.jar"'

                }

            }
        }
        stage('Deploy to QA') {
            when {
                expression {
                    return params.ENVIRONMENT == 'QA'
                }
            }
            steps {
                script {
                    // Copy the JAR to the remote server using SCP
                    sh 'scp target/spring-boot-tutorial-0.0.1-SNAPSHOT.jar ubuntu@54.234.80.65:/home/ubuntu'

                    // SSH into the server and kill the current process
                    sh 'ssh -tt ubuntu@54.234.80.65 "sudo pkill -f spring-boot-tutorial-0.0.1-SNAPSHOT.jar"'

                }

            }
        }
        stage('Deploy to prod') {
            when {
                expression {
                    return params.ENVIRONMENT == 'PROD'
                }
            }
            steps {
                script {
                    // Copy the JAR to the remote server using SCP
                    sh 'scp target/spring-boot-tutorial-0.0.1-SNAPSHOT.jar ubuntu@18.234.219.114:/home/ubuntu'

                    // SSH into the server and kill the current process
                    sh 'ssh -tt ubuntu@18.234.219.114 "sudo pkill -f spring-boot-tutorial-0.0.1-SNAPSHOT.jar"'

                }

            }
        }
    }
}