pipeline {
    agent {
        //Using Node 16 docker image as build agent
        docker {
            image 'node:16'
        }
    }
    stages {
        stage('Install dependencies') {
          //Install necessary dependencies
            steps {
                echo 'Installing dependencies'
                sh 'npm install --save'
            }
        }
      
        stage('Snyk Security scan') {
          steps {
            echo 'Initiating Snyk security scan'
            //Install Synk to perform security scan
            sh 'npm install -g snyk'
            //Authenticating synk using token
            sh 'snyk auth 75421e8e-a9d2-43a3-a6ce-b6cb827be6b8'
            script {
                def result = sh(script: 'snyk test --severity-threshold=high', returnStatus: true)
                if (result != 0) {
                    //Pipeline is halted if error detected
                    error 'Pipeline failed'
                } 
                else {
                    echo 'No Vulnerabilities detected'
                }
            }
        }
    }


        stage('Test') {
            steps {
                //Application Testing which includes unit testing
                echo 'Testing'
            }
        } 
      
        stage('Build') {
            steps {
              //Building the application
                echo 'Building application'
            }
        }
      
        stage('Run Application') {
            steps {
              //Node.js application stated
                sh 'node app.js &'
                sh 'curl http://localhost:8080'
            }
        }
        stage('Deployment Stage') {
            steps {
                echo 'Deployemnt in progress'
            }
        }
    }
    post {
      // This block runs after pipepline stages are done
        always {
            echo 'Pipeline execution completed'
        }
      //If failure
        failure {
            echo 'Error occured during pipeline execution'
        }
      //if success
        success {
            echo 'Pipeline executed successfully'
        }
    }
}
