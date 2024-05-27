pipeline {
    tools {
        maven 'mymaven' // Ensure 'mymaven' is configured in Jenkins global tool configuration
    }
    
    agent any
    
    parameters {
       string defaultValue: 'https://github.com/Sonal0409/DevOpsCodeDemo.git', description: 'Enter the Git repository URL', name: 'gitrepo'
    }
    
    stages {
        stage('1.CloneRepo') {
            steps {
                git "${params.gitrepo}"
            }
        }
        
        stage('2.CompileCode') {
            steps {
                sh 'mvn compile'
            }
        }
        
        stage('3.CodeReview') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'mvn pmd:pmd'
                }
            }
            post {
                always {
                    archiveArtifacts allowEmptyArchive: true, artifacts: '**/target/pmd.xml', followSymlinks: false
                }
            }
        }
        
        stage('4.UnitTest') {
            steps {
                sh 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('5.Package') {
            steps {
                sh 'mvn package'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war', allowEmptyArchive: true
                }
            }
     post {
        failure {
            mail to: 'ashishdubey195@gmail.com',
                 subject: "Jenkins Build Failed: ${currentBuild.fullDisplayName}",
                 body: "Something went wrong in the build: ${env.BUILD_URL}"
        }
     }
   }
 }
}    
    
        
