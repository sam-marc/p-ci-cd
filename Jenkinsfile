pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        label 'node1'
      }
      stages {
        stage('Checkout') {
      steps {
        echo 'Building the application'
        //Define build steps here
        sh '/opt/maven/bin/mvn clean package'
      }
    }
    stage('Test') {
      agent {
        label 'node1'
      }
    steps {
      echo 'Running tests'
      //Define test steps here
      sh 'mvn test'
      stash (name: 'jenkins-ci-cd', includes: "target/*war")
    }
    }
    stage('Deploy') {
      agent {
        label 'node2'
      }
      steps {
        echo 'Deploying the application'
        //Define deployment steps here
        unstash 'jenkins-ci-cd'
        sh "sudo rm -rf ~/apache*/webapp/*.war"
        sh "sudo mv target/*.war ~/apache*/webapps/"
        sh "sudo systemctl daemon-reload"
        sh "sudo ~/apache-tomcat-7.0.94/bin/shutdown.sh && sudo ~/apache-tomcat-7.0.94/bin/startup.sh"
        }
        }
    }
 post {
        always {
            // Send email notification on completion
            emailext (
                body: "Check console output at $BUILD_URL to see results,",           
                subject: "Jenkins Build ${currentBuild.currentResult} jenkins-ci-cd",
                to: "sam883marc@gmail.com, marcussamuel883@gmail.com", 
                mimeType: 'text/html'
            )
        }
    }
}


