pipeline {
    agent {
       node {
    		label 'ec2vm'
    		}
    }

     options    {
                timestamps()
                buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2'))
                timeout(time: 120, unit: 'MINUTES')
                disableConcurrentBuilds()
                }

    parameters {
            string(name: 'appBranch', defaultValue: 'master', description: "Application Branch name of the Repo")
            string(name: 'gitURL', defaultValue: 'https://github.com/myk8srepo/aws-b2-maven-app.git', description: "Pass the Maven Repo Source Code GIT URL")
                }            


    stages {
        stage('checkout') {
            steps {
                git branch: appBranch, url: https://github.com/myk8srepo/aws-b2-maven-app.git
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {

                sh 'docker build . -f Dockerfile -t image1/awsmavendemo:latest'

            }
        }  

        stage('Docker Login') {
            steps {

            withCredentials([usernamePassword(credentialsId: 'dockerlogin', passwordVariable: 'dockerpwd', usernameVariable: 'dockeruser')]) {
    
 				   sh 'docker login -u ${dockeruser} -p ${dockerpwd}'
				}

            }
        }              

        stage('Docker Push') {
            steps {
                sh 'docker push pylifedevops/awsmavendemo:latest'

            }

        } 

        stage ('K8SManiFest-Checkout') {
                steps {

                    git 'https://github.com/pythonlifedevops/aws-k8s-cicd.git'

                }
            }

        stage ('Kubernetes AutoDeployment') {
  

                steps {

                    dir('manifests')
                     {

                    sh 'ls -l'
                    // kubectl apply -f <fileName>.yml
                    sh 'kubectl --server=https://8CD576F8035B278F3CD60DFF812C8679.gr7.us-east-1.eks.amazonaws.com --insecure-skip-tls-verify --token="eyJhbGciOiJSUzI1NiIsImtpZCI6InFjNjJBY2U1X2taWklza0JHdVVscHRSc3Y4NnVEQmRHYUROejlUSzhQZk0ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjJiYTNhYmM2LTE4ODAtNDBjMi05ZTdkLTg2Y2QyOTRkZGNmMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.mMbnFdf1vZR_LHSl8dkbewmLODXFxjvOmcA276nJkg9RzRt3DSAHvfRFoLTLv-gRYpgq866D4SNFcMC2CYKljL7-6DB7F_OVRaPoXhd6w8nqGXVXJrURXuqBkoPzyU856oQuCKLKVbWl3oA93wyBRqLNlorPOz311aU3dIzBmTTaf6b7VlX-ZsmVjDOa7BPE7_1agVpXTR2ZPJ1T4gMnGP1-v6TN9FvCRDj-zTlJQzl4G5XLm8wlKWgxDtFfNFwtPK7xSF-F-kxaogZLKDeXV2-rfLWxQw19FHxhYn-SQE3bDJ6FlattECFyBRGiJmyTgxn4uepCx306apoc3HN0Pw" apply -f deployment.yml'

                    sh 'kubectl --server=https://8CD576F8035B278F3CD60DFF812C8679.gr7.us-east-1.eks.amazonaws.com --insecure-skip-tls-verify --token="eyJhbGciOiJSUzI1NiIsImtpZCI6InFjNjJBY2U1X2taWklza0JHdVVscHRSc3Y4NnVEQmRHYUROejlUSzhQZk0ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjJiYTNhYmM2LTE4ODAtNDBjMi05ZTdkLTg2Y2QyOTRkZGNmMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.mMbnFdf1vZR_LHSl8dkbewmLODXFxjvOmcA276nJkg9RzRt3DSAHvfRFoLTLv-gRYpgq866D4SNFcMC2CYKljL7-6DB7F_OVRaPoXhd6w8nqGXVXJrURXuqBkoPzyU856oQuCKLKVbWl3oA93wyBRqLNlorPOz311aU3dIzBmTTaf6b7VlX-ZsmVjDOa7BPE7_1agVpXTR2ZPJ1T4gMnGP1-v6TN9FvCRDj-zTlJQzl4G5XLm8wlKWgxDtFfNFwtPK7xSF-F-kxaogZLKDeXV2-rfLWxQw19FHxhYn-SQE3bDJ6FlattECFyBRGiJmyTgxn4uepCx306apoc3HN0Pw" apply -f service.yml'
                    
                    sh 'kubectl --server=https://8CD576F8035B278F3CD60DFF812C8679.gr7.us-east-1.eks.amazonaws.com --insecure-skip-tls-verify --token="eyJhbGciOiJSUzI1NiIsImtpZCI6InFjNjJBY2U1X2taWklza0JHdVVscHRSc3Y4NnVEQmRHYUROejlUSzhQZk0ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjJiYTNhYmM2LTE4ODAtNDBjMi05ZTdkLTg2Y2QyOTRkZGNmMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.mMbnFdf1vZR_LHSl8dkbewmLODXFxjvOmcA276nJkg9RzRt3DSAHvfRFoLTLv-gRYpgq866D4SNFcMC2CYKljL7-6DB7F_OVRaPoXhd6w8nqGXVXJrURXuqBkoPzyU856oQuCKLKVbWl3oA93wyBRqLNlorPOz311aU3dIzBmTTaf6b7VlX-ZsmVjDOa7BPE7_1agVpXTR2ZPJ1T4gMnGP1-v6TN9FvCRDj-zTlJQzl4G5XLm8wlKWgxDtFfNFwtPK7xSF-F-kxaogZLKDeXV2-rfLWxQw19FHxhYn-SQE3bDJ6FlattECFyBRGiJmyTgxn4uepCx306apoc3HN0Pw" rollout restart deployment/java-app-deployment'
                    
                    echo "kubernetes deployment is done"

                    sh 'kubectl get deployments'
                    sh 'sleep 100 ; kubectl get services'
                    
                    }

                    }
                }
        stage('Archive and clean workspace') {
                steps {
                    
                    //archive 'target/demo*.jar'
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                    cleanWs()
                }

            }              

    }
}
