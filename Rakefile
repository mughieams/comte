#encoding: utf-8
require 'html-proofer'
require 'rake/clean'
require 'jekyll'

Rake.application.options.trace = true

task :clean do
    puts 'Cleaning up _site...'.bold
    Jekyll::Commands::Clean.process({})
end

desc "Serve the site"
task :serve do
    puts 'Serving your website...'.bold
    sh 'bundle exec jekyll serve'
end

desc 'Build the site'
task build: [:clean] do
    puts 'Building your website...'.bold
    config = Jekyll.configuration({
      title: ENV["TITLE"],
      google_analytics: ENV["GOOGLE_ANALYTICS"],
      weglot_translation: ENV["WEGLOT_TRANSLATION"],
    })
    puts config
    site = Jekyll::Site.new(config)
    Jekyll::Commands::Build.build(site, config)
    puts
end

desc 'Test the site, depends on build'
task :test_html do
  puts 'Testing your website with HTMLProofer...'.bold
  options = {
    :allow_hash_href => true,   # don't break on <a href="#">
    :assume_extension => true,  # (true) for extensionless paths
    :check_html => true,
    :empty_alt_ignore => true,
    :check_favicon => true,
    :check_img_http => true,
    :http_status_ignore => [
      999, # LinkedIn throttling errors
      403, # Google scholar errors thrown from Travis (links here will be public anyway)
    ],
    :typhoeus => {
      :connecttimeout => 20,
      :timeout => 60,
      # avoid strange SSL errors: https://github.com/gjtorikian/html-proofer/issues/376
      :ssl_verifypeer => false,
      :ssl_verifyhost => 0
    }
  }
  HTMLProofer.check_directory("./_site", options).run
  puts 'Your website is now tested!'
end

desc 'Test the post structure to be sure no links to the site break'
task :test_structure do
  puts 'Testing post structure...'.bold
  file = File.absolute_path('./_site/about/index.html')
  if !File.exists?(file)
    $stderr.puts "Error: Folderstructure has changed!".bold
    exit
  end
  puts 'The post structure is now tested!'
end
