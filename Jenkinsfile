pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        
        sh 'docker build -t my-flask .'
        sh 'docker tag my-flask $DOCKER_BFLASK_IMAGE'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run my-flask python -m pytest app/tests/'
      }
    }
    
    stage('Deploy') {
    steps {
        withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerPassword')]) {
            sh "docker login -u sayeda77 -p ${dockerPassword}"
		}
        sh 'docker push sayeda77/my-flask'
    }
}

  }
  post {
    always {
      sh 'docker rm -f mypycont'
      sh 'docker run --name mypycont -d -p 3000:5000 my-flask'
    }
  }
stage('Gmail'){
	steps
	{
		emailext body: "*${currentBuild.currentResult}:* Job Name: 
                ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More 
                information at: ${env.BUILD_URL}",
		subject: 'Declarative Pipeline Build Status',
		to: 'aasifamushu@gmail.com'
     } 
}
}
