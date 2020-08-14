pipeline {
    agent any

    tools {
        maven 'Maven 3.3.9'
    }

    stages {
        stage ('Full Test Coverage') {
            steps {
                echo 'Running Mutation & Code Coverage Tests'
                sh 'mvn clean install org.pitest:pitest-maven:mutationCoverage'
                pitmutation killRatioMustImprove: false,
                minimumKillRatio: 50.0,
                mutationStatsFile: 'target/pit-reports/**/mutations.xml'
                publishCoverage adapters: [istanbulCoberturaAdapter('target/site/cobertura/*.xml')]
                publishCoverage adapters: [jacocoAdapter('target/site/jacoco/*.xml')]
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
    }
}