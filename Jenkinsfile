pipeline {
    agent { label 'jdk11-mvn3.8.4' }
     options {
    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '2'))
  }
    // triggers { 
    //     cron('5 * * * *')
    //     pollSCM('*/5 * * * *')
    // }
    parameters {
        // string(name: 'MAVEN_GOAL', defaultValue: 'clean', description: 'This is maven goal' )
        choice(name: 'MAVEN_GOAL', choices: ['clean', 'compile', 'package', 'clean install -Dmaven.test.skip=true'], description: 'This is maven goal')
        choice(name: 'BRANCH_TO_BUILD', choices: ['patch-1', 'master', 'scripted'], description: 'Branch to build')
    }
    stages {
        stage('scm') {
            steps {
          
                git url: 'https://github.com/svabhi/rock-paper-scissors.git', branch: "${params.BRANCH_TO_BUILD}"
            }
        }
        stage('build') {
            steps {
             
                sh "/usr/local/apache-maven-3.8.4/bin/mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('deploy to apache server') {
            steps {
             
               deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: ' http://52.140.1.81:8080/')], contextPath: 'rps', war: '**/*.war'
            }
            
        }
    }
    
}