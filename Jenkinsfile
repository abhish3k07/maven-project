pipeline
{
agent {
  label 'jenkins-node-1'
}

tools {
  maven 'my_maven'
}

stages{

    stage('build')
    {
        steps {
            sh 'mvn clean package'
        }

    }

    stage('test')
    {
        parallel{
            stage('testA')
            {
                steps{
                    echo "This is test A"
                }
                
            }

            stage('testB')
            {
                steps{
                    echo "This is test B"
                }
                
            }
        }

        post {
        success {
            archiveArtifacts artifacts: '**/target/*.war'
                }
             }
    }   

}

}