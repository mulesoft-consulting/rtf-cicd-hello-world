pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    BG = "ATT Product Day"
    WORKER = "Micro"
  }
  stages {
    stage('Build') {
      steps {
            sh 'mvn -f pom.xml -B -U -e -V clean -DskipTests package'
      }
    }

    stage('Test') {
      steps {
          sh "mvn -f pom.xml test"
      }
    }

    stage('Deploy to Exchange') {
      steps {
          sh 'mvn -f pom.xml deploy'
      }
    }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        TARGET = 'rtf-on-azure'
      }
      steps {
            sh 'mvn deploy -DmuleDeploy -Drtf.environment="$ENVIRONMENT" -Drtf.target="$TARGET"'
      }
    }
  }
}