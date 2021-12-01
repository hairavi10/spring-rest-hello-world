pipeline { 
    agent{ label "MASTER" }
    triggers{ pollSCM('* * * * *') }
    stages{
        stage ('Code Download From SCM'){
            steps{
                git 'https://github.com/hairavi10/spring-rest-hello-world.git'
            }
        }
        stage('Deploy_Server_setup'){
            steps{
                sh "ansible -m ping -i $WORKSPACE/inventory all"
                sh "ansible-playbook -i $WORKSPACE/inventory $WORKSPACE/myplaybook.yml --syntax-check"
                sh "ansible-playbook -i $WORKSPACE/inventory $WORKSPACE/myplaybook.yml"
            }            
        }
        stage('Deploy'){
            agent{ label "maven_server"}
            steps{
                sh "cd /home/jenkins/cloudeq-spring/" 
                sh "mvn clean package"  
                sh "cp /home/jenkins/cloudeq-spring/target/*.jar /home/jenkins/"
                sh "nohup java -jar /home/jenkins/*.jar &"
                           
            }
        } 
    }
}