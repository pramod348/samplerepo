pipeline{
    agent any

    tools {
         maven 'MAVEN_HOME'
         jdk 'JAVA_HOME'
    }

    stages{
        stage('pre-build step') {
            steps {
		sh '''
                echo "Pre Build Step"
		'''
	    }
	}
        stage('Git Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/pramod348/samplerepo.git']])
            }
        }
        stage('build'){
            steps{
               sh '''
                mvn package
                '''
            }
        }
        stage ('Unit Test') {
	        steps {
                echo 'Running Unit Testing'
                sh '''
                mvn test
                '''
             }
         }
  
        stage ('Static Code Analysis') {
             environment {
             scannerHome = tool 'SONAR_HOME'
             }
             steps {
                echo 'Running Static Code Analysis'
                 withSonarQubeEnv('SONAR_HOME') {
                 sh '${scannerHome}/bin/sonar-scanner'
                 }
            }
        }
	stage('Jfrog Artifact Upload') {
            steps {
              rtUpload (
                serverId: 'jfrog',
                spec: '''{
                      "files": [
                        {
                          "pattern": "*.war",
                           "target": "local-snapshot-repo"
                        }
                    ]
                }'''
              )
          }
        }
        stage ('Tomcat Deployment') {
           steps {
             script {
                 deploy adapters: [tomcat7(credentialsId: 'tomcatcred', path: '', url: 'http://65.0.179.42:8080/')], contextPath: 'webapp', war: '**/*.war' 
                    }
                  }
           }
         stage('post-build step') {
            steps {
		sh '''
                echo "Successfull Pipeline"
		'''
	    }
	}
    
     }
}
