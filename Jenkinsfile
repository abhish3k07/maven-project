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

        post {
        success {
            archiveArtifacts artifacts: '**/target/*.war'
                }
             }

    }

}

}