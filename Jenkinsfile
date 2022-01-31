pipeline {
    agent any
    
    stages {
        stage('Setup parameters') {
           steps {
               script {properties([parameters([choice(choices: 'master\nfeature', description: 'select the branch to build ', name: 'branch')])])}
           }
       }
       stage('Validate') {
            steps {
                echo 'Validate Code'
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'mvn test'
            }
        }
/*        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn clean sonarr:sonarr'
                }
                echo 'Sonar Analysis done: Results at Sonar Server'
            }
          }
*/
        stage('Package') {
            steps {
                echo 'Packaging....'
                sh 'mvn package'
            }
        }
        stage('Deploy to Nexus') {
            steps {
                echo ' Pushing artifact to Nexus repo'
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: '1',
                        classifier: '',
                        file: 'target/WebAppCal-1.0.war',
                        type: 'war'
                    ]
                ],
                    credentialsId: 'Nexus',
                    groupId: 'com.web.cal',
                    nexusUrl: '54.173.89.0:8081',
                    nexusVersion: 'nexus2',
                    protocol: 'http',
                    repository: 'Releases',
                    version: '1.0'
            }
        }

    }
}


