task :default do
  exec "jekyll serve --watch"
end

task :deploy do
  sh "rm -rf _site"
  sh "jekyll build"
  sh "scp -v -r _site/* qrush@tilde.club:public_html"
end
