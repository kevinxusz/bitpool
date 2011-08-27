require "bundler/gem_tasks"
require 'rspec/core/rake_task'

desc 'Default: run specs.'
task :default => :spec

desc "Run specs"
RSpec::Core::RakeTask.new

desc "start bitpool on a webrick server"
task :server do
  require 'bitpool'
  require 'rack'
  Rack::Server.new(:app => proc { |env| Bitpool::Server.new.call(env) }, :Port => ENV['PORT'] || 3002).start
end

desc "Migrate the database through scripts in db/migrate. Target specific version with VERSION=x"
task :migrate do
  require 'active_record'
  dbfile = File.expand_path("spec/fixtures/db.sqlite3", File.dirname(__FILE__))
  FileUtils.rm dbfile if File.file?(dbfile)
  ActiveRecord::Base.establish_connection(:adapter => "sqlite3", :database => dbfile)
  ActiveRecord::Migrator.migrate('migrations', ENV["VERSION"] ? ENV["VERSION"].to_i : nil )
end
