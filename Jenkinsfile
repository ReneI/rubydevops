pipeline {
agent none
  stages {
    stage('build') {
        agent { 
          docker {
            image 'ruby:2.6.1' } 
        }

      steps {
        sh 'gem install bundler -v 2.0.1'
        sh 'bundle install'
      }
    }
    
 
      stage('lint') {
                agent { docker { image 'ruby:2.6.1' } }

      steps {
        sh 'bundle exec rubocop \
--require rubocop/formatter/checkstyle_formatter \
--format RuboCop::Formatter::CheckstyleFormatter -o reports/xml/checkstyle-result.xml \
--format html -o reports/html/index.html || true'
      }
     
    }
    
    
    
       
    stage('test') {
              agent { docker { image 'ruby:2.6.1' } }

      steps {
        sh 'rake'
      }
     
    }
      
            stage('Static Analisis') {
                agent {
            docker { image 'openjdk:8' }
        }
                   steps {
               
                       script {
                          def scannerHome = tool 'SonarQubeScanner3';
                          withSonarQubeEnv("SonarQube") {
                          sh "${scannerHome}/bin/sonar-scanner"
                                       }
                               }
                           }
                        }

   
    
    
    
            stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    
  }
  
    post {
        success {
            slackSend color: 'good', message: "SUCCESS: ${JOB_NAME} ${BUILD_NUMBER}"
        }
        failure {
            slackSend color: '#ea0017', message: "FAILURE: ${JOB_NAME} ${BUILD_NUMBER}. See the results here: ${BUILD_URL}"
        }
        unstable {
            slackSend color: '#ffb600', message: "UNSTABLE: ${JOB_NAME} ${BUILD_NUMBER}. See the results here: ${BUILD_URL}"
        }
    }
  
}

