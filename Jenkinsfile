pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to QA') {
            steps {
                sh '''
                WAR_FILE=$(ls target/*.war)
                APP_NAME="proje2e"
                TOMCAT_URL="http://localhost:8082/manager/text"
                USER="deploy"
                PASS="deploy123"

                echo "Backup old WAR"
                mkdir -p backup/qa/
                cp $WAR_FILE backup/qa/${APP_NAME}_$(date +%F_%T).war || true

                echo "Deploying to QA Tomcat..."
                curl -u $USER:$PASS -T $WAR_FILE "$TOMCAT_URL/deploy?path=/$APP_NAME&update=true"
                '''
            }
        }
    }
}
