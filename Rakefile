#
# Adapted from: https://github.com/imathis/octopress
#

require "rubygems"

gen_dir    = "_site"
deploy_dir = "_deploy"

desc "Deploy to Github Pages"
task :deploy do
  puts "## Deploying to Github Pages"

  puts "## Pulling any updates from Github Pages "
  cd "#{deploy_dir}" do
    system "git pull"
  end

  (Dir["#{deploy_dir}/*"]).each { |f| rm_rf(f) }

  puts "## Copying #{gen_dir} to #{deploy_dir}"
  cp_r "#{gen_dir}/.", deploy_dir

  cd "#{deploy_dir}" do
    system "git add -A"

    message = "Site updated at #{Time.now.utc}"
    puts "## Commiting: #{message}"
    system "git commit -m \"#{message}\""

    puts "## Pushing generated #{deploy_dir} website"
    system "git push origin master"

    puts "## Deploy Complete!"
  end
end
