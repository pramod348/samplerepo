pipeline{
    agent {
      node {
	label 'slave_1'
    }
}

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
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github access', url: 'https://github.com/GoudSagar/Hello-World-Code.git']]])
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
             scannerHome = tool 'SONAR_SCANNER'
             }
             steps {
                echo 'Running Static Code Analysis'
                 withSonarQubeEnv('SONAR_HOME') {
                 sh '${scannerHome}/bin/sonar-scanner'
                 }
            }
        }
        stage ('Tomcat Deployment') {
           steps {
             script {
                 deploy adapters: [tomcat7(credentialsId: 'tomcat-credentials', path: '', url: 'http://18.119.99.37:8080')], contextPath: '/webapp-app', onFailure: false, war: 'webapp/target/webapp.war' 
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
