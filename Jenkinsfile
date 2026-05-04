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
                bat "${tool 'Maven3'}\\bin\\mvn.cmd clean package -DskipTests"
            }
        }


        stage('Deploy') {
            steps {
                // Call Ansible from WSL
                sh '''
                source ~/ansible-env/bin/activate
                cd ~/midterm
                ansible-playbook -i inventory.ini deploy.yml
                '''
            }
        }
    }

    post {
        success {
            emailext(
                to: 'e20220753@gmail.com',
                subject: 'Springboot_project-Chimoeng Build Success',
                body: """Build succeeded!
Website deployed at: http://152.42.188.57/Midterm-2026/chi_sav_moeng
Build URL: ${BUILD_URL}
Build Number: ${BUILD_NUMBER}"""
            )
        }
        failure {
            emailext(
                to: 'e20220753@gmail.com',
                subject: 'Springboot_project-Chimoeng Build Failed',
                body: """Build failed!
See Jenkins console: ${BUILD_URL}
Build Number: ${BUILD_NUMBER}"""
            )
        }
    }
}
