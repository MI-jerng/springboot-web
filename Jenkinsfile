pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MI-jerng/springboot-web.git'
            }
        }

        stage('Build') {
            steps {
                // Stop any old Gradle daemons and clean before build
                bat "gradlew.bat --stop"
                bat "gradlew.bat clean build -x test"
            }
        }

        stage('Deploy') {
            steps {
                // Run Ansible inside WSL
                bat '''
                wsl bash -c "source ~/ansible-env/bin/activate && cd ~/midterm && ansible-playbook -i inventory.ini deploy.yml"
                '''
            }
        }
    }

    post {
        success {
            emailext(
                to: 'e20220753@gmail.com',
                subject: 'Spring Boot Build Success',
                body: """Build succeeded!
Website deployed at: http://178.128.93.188/Midterm-2026/chi_sav_moeng
Build URL: ${BUILD_URL}
Build Number: ${BUILD_NUMBER}"""
            )
        }
        failure {
            emailext(
                to: 'e20220753@gmail.com',
                subject: 'Spring Boot Build Failed',
                body: """Build failed!
See Jenkins console: ${BUILD_URL}
Build Number: ${BUILD_NUMBER}"""
            )
        }
    }
}
