pipeline {
    agent any

    stages {
        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('FrontEnd/travel-bucketlist') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                bat """
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\111" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\111"
                )

                mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\111"
                xcopy /E /I /Y FrontEnd\\travel-bucketlist\\dist\\* "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\111\\"
                """
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('BackEnd/travel-bucketlist-backend') {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                bat """
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\222.war" (
                    del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\222.war"
                )
                
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\222" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\222"
                )

                copy "BackEnd\\travel-bucketlist-backend\\target\\222.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\222.war"
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed. Check Jenkins logs.'
        }
    }
}
