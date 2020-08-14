pipeline {
    agent any

    tools {
        maven 'Maven 3.3.9'
    }

    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage ('Build') {
            steps {
                echo 'Building Clean Version & Running Tests'
                sh 'mvn clean install'
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
        stage ('Testing') {
            steps {
                echo 'Running Tests'
                sh 'mvn clean install org.pitest:pitest-maven:mutationCoverage'
                pitmutation killRatioMustImprove: false,
                minimumKillRatio: 50.0,
                mutationStatsFile: 'target/pit-reports/**/mutations.xml'
                }
        }
    }
}