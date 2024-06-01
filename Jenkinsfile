pipeline{

    agent {
        label 'windows'
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
                        bat 'docker build -t mf-second_app:latest .'
                    }
                }
            }             
             
     }   

}