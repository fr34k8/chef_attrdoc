require 'bundler/gem_tasks'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:test) do |t|
  t.rspec_opts = [].tap do |a|
    a.push('--color')
    a.push('--format progress')
  end.join(' ')
end

task :default => [:test]