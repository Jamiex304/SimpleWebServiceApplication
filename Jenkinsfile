pipeline {
    agent any

    tools {
        maven 'Maven 3.3.9'
    }

    stages {
        stage ('Test And Coverage') {
            steps {
                echo 'Running Clean Install With Mutation'
                sh 'mvn clean install org.pitest:pitest-maven:mutationCoverage'
                echo 'Creating Jacoco Coverage Report'
                publishCoverage adapters: [jacocoAdapter('target/site/jacoco/*.xml')]
                echo 'Creating Mutation Coverage Report'
                pitmutation killRatioMustImprove: false,
                minimumKillRatio: 50.0,
                mutationStatsFile: 'target/pit-reports/**/mutations.xml'
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
    }
}