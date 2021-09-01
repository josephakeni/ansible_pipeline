pipeline{
    agent any
    environment{
          AWS_ACCESS_KEY_ID= credentials ('aws_access_key_id') 
          AWS_SECRET_ACCESS_KEY=credentials ('aws_secret_access_key') 
          AWS_DEFAULT_REGION='eu-west-1' 
        }
    stages{
        stage("List repo content"){
            steps{
                echo "========Listing repo content========"
                sh 'ls -ltr'
                sh 'sudo chmod +x inventory/dev/ec2*'
                sh 'ls -ltr inventory/dev/'
                sh '''
                sudo pip install boto
                export ANSIBLE_HOST_KEY_CHECKING=False
                export ANSIBLE_HOSTS=/ansible_pipeline/inventory/dev/ec2.py
                export EC2_INI_PATH=/ansible_pipeline/inventory/dev/ec2.ini
                ./inventory/dev/ec2.py --list
                '''
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
        stage("Check if playbook is ok"){
            steps{
                echo "========executing playbook check========"
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible_ssh', keyFileVariable: '')]) {
                sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/dev/ec2.py playbooks/direction_app.yaml --check; pwd'
            }
            }
        }

        stage ("Get Approval") {
            steps {
                script {
                    def userInput = input(id: 'confirm', message: 'Run Playbook ?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Run Ansible Playbook', name: 'confirm'] ])
                }
            }
        }

        stage("Run the playbook command "){
            steps{
                echo "========executing playbook command========"
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible_ssh', keyFileVariable: '')]) {
                sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/dev/ec2.py playbooks/direction_app.yaml'
            }
            }
        }
    }
    // post{
    //     always{
    //         echo "========always========"
    //     }
    //     success{
    //         echo "========pipeline executed successfully ========"
    //         mail to: 'josephakeni@gmail.com',
    //         subject: "Ansible pipeline executed successfully: ${currentBuild.fullDisplayName}",
    //         body: "Everything is correct with ${env.BUILD_URL}"
    //     }
    //     failure{
    //         echo "========pipeline execution failed========"
    //         mail to: 'josephakeni@gmail.com',
    //         subject: "Ansible pipeline execution Failed: ${currentBuild.fullDisplayName}",
    //         body: "Something is wrong with ${env.BUILD_URL}"
    //     }
    // }
}