require "rake"
require 'rake/gempackagetask'
require 'rake/contrib/rubyforgepublisher'
require 'rake/clean'
require 'rake/testtask'
require 'rake/rdoctask'

desc "Runs the Rspec suite"
task(:default) do
  run_suite
end

desc "Runs the Rspec suite"
task(:spec) do
  run_suite
end

desc "Copies the trunk to a tag with the name of the current release"
task(:tag_release) do
  tag_release
end

desc "Run GemInstaller"
task(:geminstaller) do
  require 'rubygems'
  require 'geminstaller'
  GemInstaller.run('--sudo')
end

desc "CruiseControl.rb task"
task :cruise => [:geminstaller, :default]

def run_suite
  dir = File.dirname(__FILE__)
  system("ruby #{dir}/spec/spec_suite.rb") || raise("Spec Suite failed")
end

PKG_NAME = "lsof"
PKG_VERSION = "0.2.0"
PKG_FILES = FileList[
  '[A-Z]*',
  '*.rb',
  'lib/**/*.rb',
  'spec/**/*.rb'
]

spec = Gem::Specification.new do |s|
  s.name = PKG_NAME
  s.version = PKG_VERSION
  s.summary = "A library utilizing the lsof unix command for process management."
  s.test_files = "spec/spec_suite.rb"
  s.description = s.summary

  s.files = PKG_FILES.to_a
  s.require_path = 'lib'

  s.has_rdoc = true
  s.extra_rdoc_files = [ "README", "CHANGES" ]
  s.rdoc_options = ["--main", "README", "--inline-source", "--line-numbers"]

  s.test_files = Dir.glob('spec/*_spec.rb')
  s.require_path = 'lib'
  s.autorequire = 'lsof'
  s.author = "Corey Innis + Brian Takita"
  s.email = "corey@pivotallabs.com;brian@pivotallabs.com;"
  s.homepage = "http://pivotallabs.com"
  s.rubyforge_project = "pivotalrb"
end

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.need_zip = true
  pkg.need_tar = true
end

def tag_release
  dashed_version = PKG_VERSION.gsub('.', '-')
  svn_user = "#{ENV["SVN_USER"]}@" || ""
  `svn cp svn+ssh://#{svn_user}rubyforge.org/var/svn/pivotalrb/lsof/trunk svn+ssh://#{svn_user}rubyforge.org/var/svn/pivotalrb/lsof/tags/REL-#{dashed_version} -m 'Version #{PKG_VERSION}'`
end