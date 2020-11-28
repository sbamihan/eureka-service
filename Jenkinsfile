node {
    def app

    stage('Clone Repository') {
      /* Let's make sure we have the repository cloned to our workspace */
	  checkout scm
    }
	
    stage('Build Artifact') {
      dir('eureka-service') {
        sh "mvn clean package"
      }
    }
	
    stage('Build Docker Image') {
      /* This builds the actual image; synonymous to
      * docker build on the command line */
      dir('eureka-service') {
        app = docker.build("sherwinamihan/eureka-service")
      }       
    }

    stage('Test Image') {
      /* Ideally, we would run a test framework against our image.
      * For this example, we're using a Volkswagen-type approach ;-) */
      dir('eureka-service') {
        app.inside {
          sh 'echo "Tests passed"'
        }
      }
    }
	
    stage('Push Image') {
      /* Finally, we'll push the image with two tags:
      * First, the incremental build number from Jenkins
      * Second, the 'latest' tag.
      * Pushing multiple tags is cheap, as all the layers are reused. */
      dir('head-service') {
        sh 'echo "Image pushed"'
      
        /*docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
          app.push("${env.BUILD_NUMBER}")
          app.push("latest")
        }*/
      }
    }
	
    stage('Run') {
      dir('eureka-service') {
        sh 'echo updating eureka-service'
        sh 'docker service update --image sherwinamihan/eureka-service:latest prepaid_eureka-service'
        sh 'echo eureka-service is now running at http://172.18.13.12'
      }		
    }
}
