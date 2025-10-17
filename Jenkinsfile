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
                    // Set homepage dynamically in package.json
                    sh '''
                    npx json -I -f package.json -e 'this.homepage="/reactfrontendapi"'
                    npm install
                    npm run build
                    '''
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.43/webapps/reactfrontendapi"
                rm -rf "$TOMCAT_WEBAPPS"
                mkdir -p "$TOMCAT_WEBAPPS"
                cp -R reactfrontend/build/* "$TOMCAT_WEBAPPS"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('springbootbackend') {
                    sh '/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-maven-3.9.11/bin/mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.43/webapps"
                rm -f "$TOMCAT_WEBAPPS/springbootbackendapi.war"
                rm -rf "$TOMCAT_WEBAPPS/springbootbackendapi"
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
