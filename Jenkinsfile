pipeline {
    agent any

    parameters {
        choice(
            choices: 'prod\ndev',
            description: 'Select the target environment',
            name: 'ENVIRONMENT'
        )
    }
    environment {
        registry = "anildockerpractice"
        imageName = "myubuntu"
        imageTag  = "latest"
        DOCKERHUB_CREDENTIALS=credentials('dockerhub_id')
    }
    stages {
        stage('gitclone') {
            steps {
               sh 'rm -rf jenkins_sample'
               sh 'git clone -b $Branch "https://github.com/anilgitpractice/jenkins_sample.git"'
            }
        }
    
    
        stage('Build and Push Docker Image') {
            when {
                expression { params.ENVIRONMENT == 'dev' }
            }
            steps {
                // Your Docker build and push steps for the DEV environment
                sh 'docker build -t $registry/$imageName:$imageTag .'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $registry/$imageName:$imageTag'
                
            }
        }

        stage('Deploy Docker Container') {
            when {
                expression { params.ENVIRONMENT == 'dev' }
            }
            steps {
                // Your Docker deployment steps for the DEV environment
                // Replace the placeholders with your actual commands
                sh 'docker stop $imageName:$imageTag || true'
                sh 'docker rm $imageName:$imageTag || true'
                sh 'docker run $registry/$imageName:$imageTag'
              
            }
        }

        stage('Deploy JAR Files') {
            when {
                expression { params.ENVIRONMENT == 'prod' }
            }
            steps {
                // Your JAR deployment steps for the PROD environment
                // Replace the placeholders with your actual commands
                sh 'scp myapp.jar user@remote-host:/path/to/destination'
                sh 'ssh user@remote-host "systemctl restart myapp"'
            }
        }
    }
}
