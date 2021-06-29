pipeline{
    agent any
    stages{
        stage('git scm'){
            steps{
                git branch: 'main', url: 'https://github.com/adarshss27/jenkins_ansible_test.git'
            }
        }
        stage('permission'){
            steps{
                sh 'chmod  -R 777 /var/lib/jenkins/workspace/hello'
            }
        }
        stage('copy'){
            steps{
                sh 'rsync -a --exclude=".*" /var/lib/jenkins/workspace/hello/*  adarsh@192.168.235.145:/home/adarsh/new_project'
            }
        }
        stage('Task -I'){
            steps{
                script{
                    def device = [
                        [ip:'192.168.235.145'],
                        [ip:'192.168.235.145']
                    ]
                    for (item in device) {
                        cmd = "'ssh adarsh@192.168.235.145 ansible-playbook -i " + "$item.ip" + ", /home/adarsh/new_project/ans_ble/gather_fact.yml'"
                        sh  "'$cmd'"         
                    }                                                          
                }
            }
        }
        stage('TASk - II'){
            steps{
                script{
                    def device = [
                        [ip:'192.168.235.145']
                    ]
                    for (item in device) {
                        cmd = "'ssh adarsh@192.168.235.145 ansible-playbook -i " + "$item.ip" + ", /home/adarsh/new_project/ans_ble/gather_fact1.yml'"
                        sh  "'$cmd'"                  
                    }                                                                            
                }
            }          
        }
    }
    post{
        always{
            deleteDir()
        }
        failure{
            slackSend (color: '#00FF00', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        success{
            slackSend (color: '#FF0000', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
}
