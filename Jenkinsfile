pipeline{ 
 
 agent any 
 stages { 
    stage('init') { 
    steps { 
        sh 'terraform init' 
    } 
    } 
    stage('plan') { 
    steps { 
        withCredentials([aws(accessKeyVariable:'AWS_ACCESS_KEY_ID', credentialsId: 'gulzat-aws-id', secretKeyVarible: 'AWS_SECRET_ACCESS_KEY')]) { 
        sh 'terraform plan ' 
        }      
    }
    }
    stage('terraform apply') { 
    steps { 
        withCredentials([aws(accessKeyVariable:'AWS_ACCESS_KEY_ID', credentialsId: 'gulzat-aws-id', secretKeyVarible: 'AWS_SECRET_ACCESS_KEY')]) { 
        sh 'terraform apply -auto-approve ' 
        } 
    } 
    }
//      stage('terraform destroy') { 
//     steps { 
//         withCredentials([aws(accessKeyVariable:'AWS_ACCESS_KEY_ID', credentialsId: 'gulzat-aws-id', secretKeyVarible: 'AWS_SECRET_ACCESS_KEY')]) { 
//         sh 'terraform destroy -auto-approve ' 
//         } 
// //     } 
//       } 
    stage('eks-config') {
    steps {
        withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        credentialsId: 'gulzat-aws-id',
        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws eks update-kubeconfig --region us-east-1 --name cluster-cluster'
        sh 'kubectl create -f simple.yml'
                }
            }
        }
 } 
}
