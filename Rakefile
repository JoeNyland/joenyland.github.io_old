require 'html-proofer'
require 'twitter'

task :default => :test

task :test => [:build] do
  HTMLProofer.check_directory('./_site',{
                                 check_favico: true,
                                 check_html: true,
                                 allow_hash_href: true
                             }).run
end

task :build => [:clean] do
  system 'bundle exec jekyll build'
end

task :clean do
  system 'bundle exec jekyll clean'
end

desc 'Tweet about a new post'
task :tweet_link_to_post , [:title, :url] do |task,args|

  # Connect to the Twitter API
  client = Twitter::REST::Client.new do |config|
    config.consumer_key        = ENV.fetch "TWITTER_CONSUMER_KEY"
    config.consumer_secret     = ENV.fetch "TWITTER_CONSUMER_SECRET"
    config.access_token        = ENV.fetch "TWITTER_ACCESS_TOKEN"
    config.access_token_secret = ENV.fetch "TWITTER_ACCESS_SECRET"
  end

  # Check that we've been provided with a title and URL
  fail 'Missing title or URL' if args[:title].nil? or args[:url].nil?

  # Confirm the message with the user to make sure they're happy with it
  puts 'This is the message that will be tweeted:'
  puts message = "I just published a blog post: “#{args['title']}”. Check it out here: #{args['url']}."
  puts 'Happy with it? Enter yes or no:'
  response = STDIN.gets.chomp

  if %w{y yes}.include? response.downcase
    # User is happy, so let's tweet it!
    client.update message
    puts 'Ok, tweeted that to your followers!'
  elsif %w{n no}.include? response.downcase
    # User has changed their mind, so don't tweet
    puts 'Not tweeting, as requested.'
  else
    # Invalid response, so
    fail 'You must answer yes or no'
  end

end
