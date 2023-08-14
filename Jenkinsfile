pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
     
      }
    }
    stage ('Publish to ECR') {
      steps {
         withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/f5e0m8x3' 
          sh 'docker build -t ugoecr .'
          sh 'docker tag ugoecr:latest public.ecr.aws/f5e0m8x3/ugoecr: ""$BUILD_ID""'
          sh 'docker push public.ecr.aws/f5e0m8x3/ecr/ugoecr: ""$BUILD_ID""'
         }
       }
    }
  }
}
