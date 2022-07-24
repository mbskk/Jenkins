pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'javahome2', url: 'https://github.com/mbskk/Jenkins.git'
            }
        }
        stage("Maven Build"){
            steps{
                
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  root@172.31.83.169/opt/apache-tomcat/webapps/
                    
                    ssh root@172.31.83.169 /opt/apache-tomcat/bin/shutdown.sh
                    
                    ssh root@172.31.83.169 /opt/apache-tomcat/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
