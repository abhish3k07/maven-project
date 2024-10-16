pipeline {
    agent {
        label 'jenkins-node-1'
    }

    parameters {
        choice choices: ['node-1', 'node-2'], name: 'select_environment'
    }

    tools {
        maven 'my_maven'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'  // Fixed the casing of "skipTests"
            }
        }

        stage('Test') {
            parallel {
                stage('Test A') {
                    agent { label 'jenkins-node-1' }
                    steps {
                        echo "This is test A"
                        sh 'mvn test'
                    }
                }

                stage('Test B') {
                    agent { label 'jenkins-node-1' }
                    steps {
                        echo "This is test B"
                        sh 'mvn test'
                    }
                }
            }

            post {
                success {
                    dir("webapp/target") {
                        stash name: "maven-build", includes: "*.war"
                    }
                }
            }
        }

        stage('Deploy Node 1') {
            when {
                expression { params.select_environment == 'node-1' }
            }
            agent { label 'jenkins-node-1' }
            steps {
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                cd /var/www/html/
                jar -xvf webapp.war
                """
            }
        }
    }
}
