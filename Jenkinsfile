node('maven-label') {
    def mvnHome
    stage('Preparation') { 
        git branch: 'master', url: 'https://github.com/sg-app-1/sg-core.git'
      
        mvnHome = tool 'maven-3.6.3'
    }
    stage('Build') {
        
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
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
