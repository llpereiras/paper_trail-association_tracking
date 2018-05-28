# frozen_string_literal: true

require "bundler"
Bundler::GemHelper.install_tasks

desc "Set a relevant database.yml for testing"
task :prepare do
  ENV["DB"] ||= "sqlite"
  FileUtils.cp(
    "spec/dummy_app/config/database.#{ENV['DB']}.yml",
    "spec/dummy_app/config/database.yml"
  )
end

require "rake/testtask"
desc "Run tests on PaperTrail with Test::Unit."
Rake::TestTask.new(:test) do |t|
  t.libs << "lib"
  t.libs << "test"
  t.pattern = "test/**/*_test.rb"
  t.verbose = false

  # Enabling ruby interpreter warnings (-w) is, sadly, impractical. There are
  # too many noisy warnings that we have no control over, e.g. caused by libs we
  # depend on.
  t.warning = false
end

require "rspec/core/rake_task"
desc "Run tests on PaperTrail with RSpec"
task(:spec).clear
RSpec::Core::RakeTask.new(:spec) do |t|
  t.verbose = false # hide list of specs bit.ly/1nVq3Jn
end

task :autocorrect do
  rules = [
    'FrozenStringLiteralComment',
    'Layout/EmptyLineAfterMagicComment',
    'Layout/EmptyLinesAroundBlockBody',
    'Layout/EmptyLinesAroundClassBody',
    'Layout/EmptyLinesAroundMethodBody',
    'Layout/EmptyLinesAroundModuleBody',
    'Layout/TrailingWhitespace',
    'Style/EmptyMethod',
    'Style/TrailingCommaInArguments',
  ]

  rules.each do |rule|
    `bundle exec rubocop --auto-correct --only #{rule}`
  end

  Rake::Task['rubocop'].invoke
end

require "rubocop/rake_task"
RuboCop::RakeTask.new

### TODO: Allow rubocop to fail, but still continue
desc "Default: run all available test suites"
task default: %i[prepare test spec]
