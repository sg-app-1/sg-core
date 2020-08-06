node('maven-label') {
    def mvnHome
    stage('Preparation') { 
        git branch: 'master', url: 'https://github.com/sg-app-1/sg-core.git'
        mvnHome = tool 'maven-3.6.3'
    }
    stage('sonar-scanner') {
        
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Psgcore-deploy clean sonar:sonar -Dsonar.host.url=http://10.0.0.6:9000/'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('Build') {
        
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Psgcore-deploy clean deploy'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('Results') {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
    }
}
