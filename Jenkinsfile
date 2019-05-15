node('linux'){
    stage('Test'){
        git credentialsId: 'Final', url: 'https://github.com/aisaa7339/web-test.git'
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'Final Test', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
        sh 'aws cloudformation create-stack --region us-east-1 --stack-name final-test --template-body file://docker-single-server.json --parameters ParameterKey=KeyName,ParameterValue=SEIS665-03-Spring2019-VA.pem,ParameterKey=140.209.14.79,ParameterValue=54.82.186.247/32'
    }

}
