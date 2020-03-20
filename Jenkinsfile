pipeline {
  agent { docker { image 'ruby:2.6.1' } }
  stages {
    stage('requirements') {
      steps {
        sh 'gem install bundler -v 2.0.1'
      }
    }
  
    
    stage('build') {
      steps {
        sh 'bundle install'
      }
    }
    
         stage('Quality') {
              steps {
                       
                       script {
                          def scannerHome = tool 'SonarQubeScanner3';
                          withSonarQubeEnv("SonarQube") {
                              sh 'mvn clean package sonar:sonar'
                                         }
                               }
                           }
                        }
    
    
    
            stage('Code Quality') {
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

      stage('lint') {
      steps {
        sh 'bundle exec rubocop \
--require rubocop/formatter/checkstyle_formatter \
--format RuboCop::Formatter::CheckstyleFormatter -o reports/xml/checkstyle-result.xml \
--format html -o reports/html/index.html || true'
      }
     
    }
    

    
    stage('test') {
      steps {
        sh 'rake ci:all'
      }
      post {
        always {
          junit '**/test-reports/*.xml'
        }
      }    
    }
  }
}

