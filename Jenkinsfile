pipeline {
    agent {
        label 'build-server'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application'
                sh '/opt/maven/bin/mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'mvn test'
            }
        }
        stage('Deploy') {
    agent {
        label 'deploy-server'
    }
    steps {
        echo 'Deploying the application'
        sh "sudo rm -rf /home/centos/apache*/webapp/*.war"
        sh "sudo mv /home/centos/workspace/Jenkins CI-CD/target/*.war /home/centos/apache-tomcat-7.0.94/webapps/"
        sh "sudo systemctl daemon-reload"
        sh "sudo ~/apache-tomcat-7.0.94/bin/shutdown.sh && sudo ~/apache-tomcat-7.0.94/bin/startup.sh"
    }
}
  }
    post {
        success {
            mail to: "towehcorina@gmail.com",
            subject: "Jenkins CI-CD Successful",
            body: "Jenkins CI-CD was successfully built, tested, and deployed"
        }
        failure {
            mail to: "towehcorina@gmail.com",
            subject: "Jenkins CI-CD Failed",
            body: "Jenkins CI-CD failed, please investigate"
        }
    }
}

       
     
