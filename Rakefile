require "erubis"
require "date"
require 'pp'

desc "generate a new agenda for the next Thursday"
task :workflow do
  erb = Erubis::Eruby.new(File.read("agenda_template.md.erb"))
  date = Date.parse("thursday")
  next_week = date + 7
  filename = date.strftime("%Y-%m-%d-agenda.md")
  File.open(filename, "w") { |fh| fh.write erb.result(binding()) }
  sh "git add #{filename}"
  puts "Now review and commit #{filename}"
end

task :default => [:workflow]
