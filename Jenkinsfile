node('linux'){
    stage('Test'){
        git credentialsId: 'Assignment12', url: 'https://github.com/aisaa7339/web-test'
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
        {
        sh 'aws cloudformation create-stack --region us-east-1 --stack-name final-test --template-body file://docker-single-server.json --parameters ParameterKey=KeyName,ParameterValue=SEIS665-03-Spring2019-VA ParameterKey=YourIp,ParameterValue=$(curl ifconfig.me)/32'
        sh 'aws cloudformation wait stack-create-complete --stack-name final-test --region us-east-1'
        sh 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1'
        env.docker1IP = sh returnStdout: true, script: 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1 --query Stacks[].Outputs[].[OutputValue] --output text'
        sshagent(['5b9e4226-b3c0-4b06-8252-22126d422bcf']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@${docker1IP} uptime'
        }

      }
    }
    
