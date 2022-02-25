pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'build done1'
        sh './gradlew clean build'
      }
    }

    stage('upload') {
      steps {
        echo 'upload done2'
        sh 'aws s3 cp build/libs/application.war s3://springboot-cicd-river/application.war --region us-east-2' 
      }
    }

    stage('deploy') {
      steps {
        echo 'deploy done3'
        sh 'aws elasticbeanstalk create-application-version --region us-east-2 --application-name springboot-cicd-river --version-label ${BUILD_TAG} --source-bundle S3Bucket="springboot-cicd-river",S3Key="application.war"' 
        sh 'aws elasticbeanstalk update-environment --region us-east-2 --environment-name Springbootcicdriver-env-1 --version-label ${BUILD_TAG}'
        slackSend (color: '#FF0000', message: "빌드 완료 : 성공 실패는 따로 확인 ! STATUS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    }
  }
}