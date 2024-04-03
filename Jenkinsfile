pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        label 'node1'
      }
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
    success {
        echo 'Pipeline succeeded! Send success notification.'
        emailext subject: "Success: ${currentBuild.fullDisplayName}",
                  body: "Build was successful. Congratulations!",
                  to: 'sam883marc@gmail.com'
    }
    failure {
        echo 'Pipeline failure! Send failure notification.'
        emailext subject: "Failure: ${currentBuild.fullDisplayName}",
                  body: "Something went wrong. Please check the build logs.",
                  to: 'sam883marc@gmail.com'
    }
}



