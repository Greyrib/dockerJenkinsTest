pipeline {
  agent any

  environment {
    IMAGE_NAME = "https://github.com/Greyrib/dockerJenkinsTest/blazor-app:${env.BUILD_NUMBER}"
    DOCKER_REGISTRY_CREDENTIALS_ID = "GreyGitPAT"
  }

  stages {
    stage('Checkout') {
      steps {
        //checkout scm // OLD VARIANT BAD
        // **Explicitly specify the main branch**
        checkout([$class: 'GitSCM',
          branches: [[name: '***/main**']], // **Changed from '*/master' to '*/main' here**
          userRemoteConfigs: [[url: 'https://github.com/Greyrib/dockerJenkinsTest.git']] // **Ensure this URL is correct for your repo**
        ])
        extensions: [[$class: 'CleanBeforeCheckout']] // Ensure a clean fetch
      }
    }

    stage('Build Docker Image') {
      steps {
        sh "docker build -t $IMAGE_NAME ."
      }
    }

    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDENTIALS_ID}", usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh """
            echo $PASS | docker login -u $USER --password-stdin yourregistry
            docker push $IMAGE_NAME
          """
        }
      }
    }

    stage('Deploy') {
      steps {
        sh './deploy.sh' // Customize: can be kubectl apply, helm, etc.
      }
    }
  }
}
