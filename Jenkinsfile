pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:${env.PATH}" // ensure npm & node are visible
    }

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('reactfrontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.XX/webapps/reactfrontendapi"
                rm -rf "$TOMCAT_WEBAPPS"
                mkdir -p "$TOMCAT_WEBAPPS"
                cp -R reactfrontend/dist/* "$TOMCAT_WEBAPPS"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('springbootbackend') {
                    sh '/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-maven-3.9.11/bin/mvn clean package'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.XX/webapps"
                rm -f "$TOMCAT_WEBAPPS/springbootbackend.war"
                rm -rf "$TOMCAT_WEBAPPS/springbootbackend"
                cp springbootbackend/target/*.war "$TOMCAT_WEBAPPS/"
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}