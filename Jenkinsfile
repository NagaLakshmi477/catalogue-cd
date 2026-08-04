pipeline {
    agent {
        label 'AGENT-1'
    }
    environment {
        appVersion = ''
        region = 'us-east-1'
        account_id = '143094436925'
        project = 'roboshop'
        component = 'catalogue'
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'appVersion', description: 'Image version of the application')
        choice(name: 'deploy_to', choices:['dev', 'qa', 'prod'], description: 'Pick any env')
    }

    stages {
        stage('Deploy') {
            steps {
                withAWS(credentials: 'aws-auth', region: 'us-east-1') {
                    sh """ 
                       aws eks update-kubeconfig --region ${region} --name ${project}-${params.deploy_to}
                        kubectl get nodes
                        kubectl apply -f 01-namespace.yaml
                        sed -i "s/IMAGE_VERSION/${params.appVersion}/g" values-${params.deploy_to}.yaml
                        helm upgrade --install $component -f values-${params.deploy_to}.yaml -n $project
                    """}
            
            }
        
        
        
        }
    
    }



}