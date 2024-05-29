pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
    

                // Build Docker image
                sh './gradlew build --no-daemon'
                sh 'docker build -t train-schedule:${BUILD_NUMBER} .'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

        // stage('Code Quality Analysis') {
        //     steps {
        //         // Install SonarQube Scanner
        //         sh 'wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip'
        //         sh 'unzip sonar-scanner-cli-4.6.2.2472-linux.zip'

        //         // Run code quality analysis
        //         sh './sonar-scanner-4.6.2.2472-linux/bin/sonar-scanner'
        //     }
        // }

        stage('Deploy') {
            steps {
     
     

                // Deploy using Docker Compose
                sh 'docker-compose -f docker-compose.yml up -d'
            }
        }

        // stage('Monitoring and Alerting') {
        //     steps {
        //         // Send email notification
        //         emailext body: 'Jenkins pipeline completed successfully.',
        //                  subject: 'Pipeline Notification',
        //                  to: 'your_email@example.com'
        //     }
        // }
    }

    post {
        always {

            sh 'docker-compose -f docker-compose.yml down'
            sh 'docker container rm $(docker container ls -aq) -f'

            // Clean up resources if needed
        }
    }
}
