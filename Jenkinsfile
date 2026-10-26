pipeline {
    agent {
        label 'node1'
    }

    environment {
        MVN = '/usr/share/maven/bin/mvn'
    }

    tools {
        maven 'MAVEN_HOME'
    }

    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }

    triggers {
        cron('0 * * * *')
    }

    parameters {
        choice(
            name: 'GOAL',
            choices: ['clean', 'clean package', 'clean install'],
            description: 'Choose preferred Maven option'
        )
    }

    stages {
        stage('Git clone') {
            steps {
                git url: 'https://github.com/teja6177/spring-petclinic.git',
                    branch: 'main'
            }
        }

        stage('Build the code') {
            steps {
                sh '"$MVN" ${GOAL}'
            }
        }

        stage('Reporting & Archiving') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        success {
            echo 'Success'
        }

        unsuccessful {
            echo 'Failed'
        }
    }
}
