node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("eshnil/test-jenkins")
    }

    stage('Test image') {
  
        docker.image('eshnil/test-jenkins:latest').withRun('-p 8787:8080') { c ->
        /* Wait until mysql service is up */
            sh '/usr/bin/wget http://10.0.0.205:8787'
        /* Run some tests which require MySQL */
            sh 'cat index.html'
    }
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'eshani-dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
