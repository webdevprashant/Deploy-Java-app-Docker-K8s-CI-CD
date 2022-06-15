pipeline {
    
    agent none
    
    
    stages {
        stage('SCM') {
            agent { label 'master'  }
            steps {
                git 'https://github.com/webdevprashant/Depploy-Java-app-Docker-K8s-CI-CD.git'
                
            }
            
        }
        
        stage('Build by Maven Package') {
                        agent { label 'master'  }
            steps {
                sh 'mvn clean package'
            }
            
        }
        
        
        stage('Build Docker OWN image') {
                        agent { label 'master'  }
            steps {
                sh "sudo docker build -t  webdevprashant/javaweb:${BUILD_NUMBER}  ."
                //sh 'whoami'
            }
            
        }
        
        
        stage('Push Image to Docker HUB') {
                        agent { label 'master'  }
            steps {
                
                withCredentials([string(credentialsId: 'Docker_hub_password', variable: 'Docker_hub_password')]) {
    // some block
                 sh "sudo docker login -u webdevprashant -p $Docker_hub_password"
}
               
              sh "sudo docker push webdevprashant/javaweb:${BUILD_NUMBER}"
            }
            
        }
        
        
        stage('Deploy webAPP in DEV Env') {
                        agent { label 'master'  }
            steps {
                sh 'sudo docker rm -f myjavaapp'
                sh "sudo docker run  -d  -p  1230:8080 --name myjavaapp   webdevprashant/javaweb:${BUILD_NUMBER}"
                //sh 'whoami'
            }
            
        }
        
        
        stage('Deploy webAPP in QA/Test Env') {
                        agent { label 'master'  }
            steps {
               
            //   sshagent(['QA_ENV_SSH_CRED']) {
            //     }
    
             // sh "ssh  -o  StrictHostKeyChecking=no ec2-user@13.233.100.238 sudo docker rm -f myjavaapp"
            // sh "ssh ec2-user@13.233.100.238 sudo docker run  -d  -p  8080:8080 --name myjavaapp   vimal13/javaweb:${BUILD_TAG}"
            sh "sudo docker rm -f myapp"
            sh "sudo docker run -d -it --name myapp -p 1235:8080  webdevprashant/javaweb:${BUILD_NUMBER}"

            }
            
        }
        
        
        //  stage('QAT Test') {
        //                 agent { label 'master'  }
        //     steps {
                
        //       // sh 'curl --silent http://13.233.100.238:8080/java-web-app/ |  grep India'
                
        //         retry(40) {
        //             sh 'curl --silent http://192.168.43.56:8080/java-web-app/ |  grep India'
        //         }
            
               
        //     }
        // }
          
        
         
         
        stage('approved') {
                        agent { label 'master'  }
            steps {
            script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                }
            
                
            }
        }
        }
        
        stage('Deploy webAPP in Prod Env') {
            agent { label 'windowsslave'  }
            steps {
            //   sshagent(['QA_ENV_SSH_CRED']) { 
                    // sh "ssh  -o  StrictHostKeyChecking=no ec2-user@13.232.250.244 sudo kubectl  delete    deployment myjavawebapp"
                    // sh "ssh  ec2-user@13.232.250.244 sudo kubectl  create    deployment myjavawebapp  --image=vimal13/javaweb:${BUILD_TAG}"
                    // sh "ssh ec2-user@13.232.250.244 sudo wget https://raw.githubusercontent.com/vimallinuxworld13/jenkins-docker-maven-java-webapp/master/webappsvc.yml"
                    // sh "ssh ec2-user@13.232.250.244 sudo kubectl  apply -f webappsvc.yml"
                    // sh "ssh ec2-user@13.232.250.244 sudo kubectl  scale deployment myjavawebapp --replicas=5"
            //   }
            bat 'cmd /c kubectl  delete    deployment myjavawebapp'
            bat 'cmd /c kubectl  create    deployment myjavawebapp  --image=webdevprashant/javaweb:${BUILD_TAG}'
            bat 'curl -o myappsvc.yml https://raw.githubusercontent.com/vimallinuxworld13/jenkins-docker-maven-java-webapp/master/webappsvc.yml'
            bat 'cmd /c kubectl  apply -f webappsvc.yml'
            bat 'cmd /c kubectl  scale deployment myjavawebapp --replicas=5'
            }
        } 
    }
     post {
         always {
             echo "You can always see me"
         }
         success {
              echo "I am running because the job ran successfully"
         }
         unstable {
              echo "Gear up ! The build is unstable. Try fix it"
         }
         failure {
             echo "OMG ! The build failed"
             mail bcc: '', body: 'hi check this ..', cc: '', from: '', replyTo: '', subject: 'job ete fail', to: 'vdaga@lwindia.com'
         }
     }
}
