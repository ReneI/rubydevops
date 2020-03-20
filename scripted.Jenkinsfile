node {
  checkout scm
 
  stage 'Build'
  def docker_image = docker.build("webmf/ruby-sinatra:${env.BUILD_ID}", '.')

  stage 'Test'
  docker_image.inside {
    try {
      sh 'rake ci:all'
    } catch (e) {
      echo 'Tests have failed!'
    } finally {
      junit 'test/reports/TEST-AppTest.xml'
    }
  }
}


