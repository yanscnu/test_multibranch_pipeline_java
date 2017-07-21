node {
    stage('scm') {
        checkout scm
    }
    
    stage('SonarQube analysis') {
        def scannerHome = tool 'sonar-scanner-3.0.3.778';
        withSonarQubeEnv {
          sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=${JOB_NAME} -Dsonar.sources=src"
        }
    }
    
    stage('Maven build') {
        def mvnHome = tool 'mvn3'
        sh "${mvnHome}/bin/mvn -B -Dmaven.test.failure.ignore verify"
    }
    
    stage('Store artifact'){
        step([$class: 'ArtifactArchiver', artifacts: '**/target/*.jar', fingerprint: true])
        step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    }
}
