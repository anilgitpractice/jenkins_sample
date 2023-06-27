pipeline {
    agent any

    parameters {
        choice(
            choices: 'prod\ndev',
            description: 'Select the target environment',
            name: 'ENVIRONMENT'
        )
        choice(
            choices: 'main\ndev',
            description: 'Select the target BRANCH',
            name: 'BRANCH'
        )

       choice(
            choices: 'myubuntu\nsample-pipe\nmyimage1',
            description: 'Select the Service Image',
            name: 'CONTAINER'
       ) 
       
    }
    environment {
        registry = "anildockerpractice"
        imageTag  = "latest"
        DOCKERHUB_CREDENTIALS=credentials('dockerhub_id')
    }    
    stages {
        stage('Git Clone') {
            steps {
               sh 'rm -rf jenkins_sample'
               sh 'git clone -b $BRANCH "https://github.com/anilgitpractice/jenkins_sample.git"'
            }
        }
        stage('Build and push Docker image') {
            when {
                expression { params.ENVIRONMENT == 'dev' }
            }
            steps {
                script {
                    sh 'echo Im before docker build'
                    def dockerRegistry = "anildockerpractice"
                     
                    def imageFullName = "${registry}/${params.CONTAINER}:${imageTag}"
                    echo imageFullName
                    //def path = "jenkins_sample"+{params.BRANCH}
                    //echo path
                   // cd path
                    docker.build(imageFullName)
                    
                    // for (def imageName in params.imageNames) {
                    // // Build and tag the image
                    // def imageFullName = "${registry}/${imageName}:${imageTag}"
                    // docker.build(imageFullName)

                    // // Push the image to Docker Hub
                    // docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                    // docker.image(imageFullName).push()
            }
          }
        }
      }
    

        // Add more stages as needed for your build and deployment process
    }
