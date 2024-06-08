pipeline{

    agent {
        label 'windows'
    }
        
        environment {

            DOCKER_CREDENTIALS_ID= 'dockerhub'
            DOCKER_IMAGE_NAME= 'samadhangapat/mf_app:latest'
            DOCKER_USERNAME='samadhangapat'
            DOCKER_PASSWORD='Samraj@10'
            
        }

        stages{
            stage('checkout'){

                steps {

                    git url: 'https://github.com/Samraj10/mf-second.git', branch: 'master'

                }

            }
            stage('build'){

                steps {

                    bat 'mvn clean package'
                }
            }
            stage('test'){

                steps {

                    bat 'mvn test'
                }

            }

            stage ('check docker version') {

                steps {

                    script {
                    // Verify Docker is available
                    bat 'docker --version'
                     }
                }

            }

            stage ('build docker image') {

                steps {

                    script {

                        def dockerFileName='Dockerfile'
                        def dockerTag='latest'
                        def dockerfilePath='D://applications//mf//mf'
                        def dockerImageName='mf-second_app'
                        bat 'cd D:/applications/mf-second/mf-second'
                        bat 'docker build -t samadhangapat/mf-second_app:latest .'
                    }
                }
            }
/*
            stage('push docker image'){

                steps {

                    script {

                        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        }
                            bat " docker push ${DOCKER_IMAGE_NAME} "

                    }

                }
            }             
*/
            stage('SSH into Ansible Server and Run Playbook') {
            steps {
                // Use the SSH Publisher plugin to run commands on the remote ser
                sshPublisher( 
                    publishers: [
                        sshPublisherDesc(
                            configName: 'ansible_server',  // Name of the SSH server configured in Jenkins
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/mf-second_deploy.yml',  // Source files to transfer (optional)
                                    execCommand: 'ansible-playbook /home/samra/ansible_work/kubernetes/mf-second_deploy.yml',  // Command to execute
                                //   remoteDirectory: '/home/samra/ansible_work',  // Remote directory (optional)                                 
                                    removePrefix: '',  // Remove prefix from transferred files (optional)
                                    execTimeout: 120000,  // Execution timeout in milliseconds (optional)
                                    usePty: true  // Use Pseudo Terminal (optional)
                                )
                            ],
                            usePromotionTimestamp: false,
                            useWorkspaceInPromotion: false,
                            verbose: true
                        )
                    ]
                )
            }
        }            
     }   

}