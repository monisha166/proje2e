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

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to DEV') {
            steps {
                sh '''
                TOMCAT="/home/abc/mavenprojects/tomcat9-dev"
                APP="proje2e"
                WAR="target/*.war"
                BACKUP="$TOMCAT/backup"
                TS=$(date +'%Y%m%d_%H%M%S')

                echo "Stopping Tomcat..."
                $TOMCAT/bin/shutdown.sh || true
                sleep 3

                mkdir -p $BACKUP
                if [ -f "$TOMCAT/webapps/$APP.war" ]; then
                    mv $TOMCAT/webapps/$APP.war $BACKUP/${APP}_${TS}.war
                fi

                rm -rf $TOMCAT/webapps/$APP
                cp $WAR $TOMCAT/webapps/$APP.war

                echo "Starting Tomcat..."
                $TOMCAT/bin/startup.sh
                '''
            }
        }
    }
}
