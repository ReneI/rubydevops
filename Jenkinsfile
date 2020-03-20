pipeline {
agent none
  stages {
   
    stage('build') {
        agent { docker { image 'ruby:2.6.1' } }
     steps {
        sh 'gem install bundler -v 2.0.1'
      }
      steps {
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
        sh 'rake ci:all'
      }
      post {
        always {
          junit '**/test-reports/*.xml'
        }
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

   
    
  }
}

