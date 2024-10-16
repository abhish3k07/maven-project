pipeline
{
agent {
  label 'jenkins-node-1'
}

parameters {
  choice choices: ['node-1', 'node-2'], name: 'select_environment'
}

tools {
  maven 'my_maven'
}

stages{

    stage('build')
    {
        steps {
            sh 'mvn clean package -Dskiptests=true'
        }

    }

    stage('test')
    {
        parallel{
            stage('testA')
            {
                agent {label 'jenkins-node-1'}
                steps{
                    echo "This is test A"
                    sh 'mvn test'
                }
                
            }

            stage('testB')
            {
                agent {label 'jenkins-node-1'}
                steps{
                    echo "This is test B"
                    sh 'mvn test'
                }
                
            }
        }

        post {
        success {
            dir("webapp/target")
            {
                stash name: "maven-build", includes: "*.war"
            }
                }
             }
    }   

    stage('deploy_node1')
    {
        when {expression {params.select_environment == 'node-1'}}
        beforeAgent true
        agent {label 'jenkins-node-1'}
        steps
        {
            dir("var/www/html")
            {
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