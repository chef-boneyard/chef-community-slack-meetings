require "octokit"
require "erb"
require "date"
require 'pp'

def github
  @github ||= Octokit::Client.new(:netrc => true)
end

DISCUSSION_LABEL = 'label:"Under Discussion"'
VOTING_LABEL = 'label:"Ready for Vote"'

QUERY = 'repo:chef/chef-rfc is:pr state:open -label:"On Hold" -label:Approved '
NEW = QUERY + "-#{DISCUSSION_LABEL} -#{VOTING_LABEL}"
DISCUSSION = QUERY + DISCUSSION_LABEL
VOTING = QUERY + VOTING_LABEL

task :workflow do
  Octokit.auto_paginate = true

  new = github.search_issues(NEW)[:items].map { |item| [item[:url], item[:title]] }
  discussion = github.search_issues(DISCUSSION)[:items].map { |item| [item[:url], item[:title]] }
  voting = github.search_issues(VOTING)[:items].map { |item| [item[:url], item[:title]] }

  erb = ERB.new(File.read("agenda_template.md.erb"), 0, "-")
  date = Date.parse("thursday")
  next_week = date + 7
  filename = date.strftime("%Y-%m-%d-agenda.md")
  File.open(filename, "w") { |fh| fh.write erb.result(binding()) }
  sh "git add #{filename}"
  puts "Now review and commit #{filename}"
end

task :default => [:workflow]
