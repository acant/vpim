require 'rake/clean'
CLOBBER.include 'pkg'

require 'bundler/gem_helper'
namespace :vpim do
  Bundler::GemHelper.install_tasks(name: 'vpim')
end

namespace :vpim_icalendar do
  Bundler::GemHelper.install_tasks(name: 'vpim_icalendar')
end

task :stamp do
  ruby 'stamp.rb > lib/vpim/version.rb'
end

task :copy_bin do
  sh 'mkdir -p bin'
  sh 'cp -v samples/reminder.rb bin/reminder'
  sh 'cp -v samples/rrule.rb bin/rrule'
  sh 'chmod +x bin/*'
end

desc 'Build all gems into the pkg directory'
task build: %i[copy_bin vpim:build vpim_icalendar:build]

desc 'Build and install all gems into system gems'
task install: %i[copy_binvpim:install vpim_icalendar:install]

namespace :install do
  desc 'Build and install all gems into system gems without network access'
  task local: %i[copy_bin vpim:install:local vpim_icalendar:install:local]
end

desc 'Creat tag and build and push all gems to rubygems.org'
task release: %i[stamp copy_bin vpim:release vpim_icalendar:release]

require 'rdoc/task'
RDoc::Task.new do |rdoc|
  rdoc.main     = 'README.rdoc'
  rdoc.rdoc_dir = 'doc'
  rdoc.rdoc_files.include(
    'README.rdoc',
    'lib/**/*.rb',
    'CHANGES',
    'COPYING',
    'README.rdoc',
    'samples/README.mutt'
  )
end

task :tags do
  sh '/sw/bin/ctags -R --extra=+f lib test'
end

require 'pathname'

task :unit_test do
  ruby 'test/test_all.rb'
end

task :example_test do
  Pathname.glob('ex_*.rb') { |filename| ruby "-w -I lib #{filename}" }
end

desc 'Run tests'
task test: %i[unit_test example_test]

task :bday do
  ruby '-I lib samples/vcf-to-ics.rb < _all.vcf | tee _bday.ics'
end

task :reminder do
  ruby '-I lib samples/reminder.rb'
end

task :outline do
  sh 'sh outline.sh > outline.txt'
end

task :dcal do
  ruby 'samples/ics-dump.rb ~/Library/Calendars/Play.ics'
  ruby 'samples/ics-dump.rb ~/Library/Calendars/Events.ics'
end

task :ics_api do
  ruby '-I lib -w -rpp ex_ics_api.rb > ex_ics_api.out'
end

task default: %i[test]
