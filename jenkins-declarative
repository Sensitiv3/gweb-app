pipeline{
  agent any 
  tools {
    maven "maven"
  }  
  stages {
    stage('1GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git branch: 'master', url: 'https://github.com/Sensitiv3/gweb-app.git'
      }
    }
    stage('2Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'testing must passed to create artifacts ' "
        sh "mvn clean package"
		}
	}
	stage('3CodeQuality'){
		steps{
			sh "echo 'Performing CodeQualityAnalysis' "
			sh "mvn sonar:sonar"
		}
	}
	stage('4uploadArtifacts'){
		steps{
			sh "mvn deploy"
		}
	}
	stage('8deploy2Prod'){
		steps{
			deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://3.94.109.74:8080/')], contextPath: null, war: 'target/*war'
		}
	}
  }
  post {
	always {
		emailext body: '''Team,

Please check build status.

Good Job''', recipientProviders: [developers()], subject: 'Success', to: 'paulsensitive@gmail.com'
	}
	success {
		emailext body: '''Team,

Deployment is successful.

Good Job''', recipientProviders: [developers()], subject: 'Success', to: 'paulsensitive@gmail.com'
	}
	failure {
		emailext body: '''Team,

Build failed, please resolve.

Good Job''', recipientProviders: [developers()], subject: 'Success', to: 'paulsensitive@gmail.com'
	}
  }
}
