pipeline {
    agent { label 'JDK-17-MVN-3.6' }
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/satya36-cpu/spring-framework-new.git',
                    branch: 'master'
            }
        }
        stage('build') { 
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
				}
                
                
        }
        
        stage('post build') {
            steps {
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
        stage('docker image build & push') {
            steps {
                sh 'docker image build -t satyabrata36/spcnow:1.0 .'
                sh 'docker image push satyabrata36/spcnow:1.0'
            }
        }
        stage('deploy on k8s') {
		    agent { label 'K8S_DEPLOY' }
			steps {
			    sh 'kubectl apply -f deployment.yaml'
			}
		}
    }
}