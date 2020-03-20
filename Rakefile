require 'rake/testtask'
require 'ci/reporter/rake/minitest'

task default: %i(test)
task minitest: 'ci:setup:minitest'

namespace :ci do
  task :all => ['ci:setup:minitest', 'test']
end

Rake::TestTask.new do |t|
  t.pattern = 'tests.rb'
  t.warning = false
  t.verbose = true
end
