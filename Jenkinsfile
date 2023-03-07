pipeline {
    agent { label 'JDK-11-MAVEN-3.6' }
    triggers { pollSCM ('* 7 * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/satya36-cpu/spring-framework-new.git',
                    branch: 'dev'
            }
        }
        stage('build') { 
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
				}
                
                
        }
        
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}