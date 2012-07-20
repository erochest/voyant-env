
# Things this doesn't do:
# * set debugging output in .htaccess and application/config/config.ini; and
# * turning on the proper plugins.

require 'date'
require 'etc'
require 'fileutils'
require 'pathname'
require 'vagrant'


COOKBOOKS = %w{
  application
  application_java
  runit
  unicorn
  passenger_apache2
  tomcat
  python
  gunicorn
  apache2
  php
}


task :default => :usage

task :usage do
  puts "You forgot to tell the computer what to do; try one of these commands:"
  system("rake -T")
end

# Execute a command in the primary VM and write its output to the screen.
def vm_ssh(env, cmd)
  puts ">>> '#{cmd}'"
  env.primary_vm.channel.execute cmd do |channel, data|
    out = $stdout
    if channel == :stderr
      out = $stderr
    end
    out.write(data)
    out.flush
  end
end

desc 'Destroys and initializes the environment.'
task :init => [
  :clobber,
  :cookbooks,
  'vagrant:up',
]

desc 'Cleans everything out of the environment.'
task :clobber => ['neatline:clobber'] do
  # Rake::Task['vagrant:chef:clean'].invoke
  Rake::Task['vagrant:destroy'].invoke
end

desc 'Downloads the cookbooks.'
task :cookbooks do
  cbdir = 'cookbooks'
  Dir.mkdir(cbdir) unless File.directory?(cbdir)
  COOKBOOKS.each do |cookbook|
    Rake::Task['vagrant:chef:cookbook'].invoke(
      "https://github.com/opscode-cookbooks/#{cookbook}",
      "#{cbdir}/#{cookbook}"
    )
    Rake::Task['vagrant:chef:cookbook'].reenable
  end
  Rake::Task['vagrant:chef:cookbook'].invoke(
    "https://github.com/erochest/voyant-cookbook",
    "#{cbdir}/voyant"
  )
  Rake::Task['vagrant:chef:cookbook'].reenable
end

require 'zayin/rake/vagrant'
Zayin::Rake::VagrantTasks.new

