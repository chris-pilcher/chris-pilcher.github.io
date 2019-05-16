require 'rubygems'
require 'rake'
require 'rdoc'
require 'date'
require 'yaml'
require 'tmpdir'
require 'jekyll'

desc "Generate blog files"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end


desc "Generate and publish blog to master branch"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    system "mv _site/* #{tmp}"
    system "git checkout -B master"
    system "rm -rf *"
    system "mv #{tmp}/* ."
    message = "Site updated at #{Time.now.utc}"
    system "git add ."
    system "git commit -am #{message.shellescape}"
    system "git push origin master --force"
    system "git checkout jekyll"
    system "echo yolo"
  end
end

desc "Only for the template project: Generate and publish blog to gh-pages"
task :'publish-to-gh-pages' => [:generate] do
  Dir.mktmpdir do |tmp|
    system "mv _site/* #{tmp}"
    system "git checkout -B gh-pages"
    system "rm -rf *"
    system "mv #{tmp}/* ."
    message = "Site updated at #{Time.now.utc}"
    system "git add ."
    system "git commit -am #{message.shellescape}"
    system "git push origin gh-pages --force"
    system "git checkout master"
    system "echo yolo"
  end
end

namespace :new do
	desc "Create the file for a new post"
	task :post do
		STDOUT.puts "Input the title of the post. It will be used for the permalink too.\nAny non-alphanumeric symbols will be converted to `-' in the permalink."

		title = STDIN.gets.chomp
		date = Time.now.strftime("%Y-%m-%d")
		time = Time.now.strftime("%H:%M:%S")
		fname = Time.now.strftime("%Y-%m-%d") + "-" + title.gsub(/^\W+|\W+$/,"").gsub(/\W+/,"-")
		File.open("_posts/#{fname}.markdown", "w") {|f|
			# Add file content
			f.puts("---",
				   "layout: post",
				   "title: \"#{title}\"",
				   "date: #{date} #{time}",
				   "categories: jekyll update",
				   "---")
		}

		STDOUT.puts "File #{fname}.markdown created in directory _posts."
	end

	desc "Create the file for a link post"
	task :link do
		STDOUT.puts "Input the title of the link. It will be used for the permalink too.\nAny non-alphanumeric symbols will be converted to `-' in the permalink."
		title = STDIN.gets.chomp

		STDOUT.puts "Input the url for the link."
		url = STDIN.gets.chomp

		date = Time.now.strftime("%Y-%m-%d")
		time = Time.now.strftime("%H:%M:%S")
		fname = Time.now.strftime("%Y-%m-%d") + "-" + title.gsub(/^\W+|\W+$/,"").gsub(/\W+/,"-")

		File.open("_posts/#{fname}.markdown", "w") {|f|
			# Add file content
			f.puts("---",
				   "layout: post",
				   "title: \"#{title}\"",
				   "date: #{date} #{time}",
				   "external-url: #{url}",
				   "---")
		}

		STDOUT.puts "File #{fname}.markdown created in directory _posts."
	end
end

task :default => :publish
