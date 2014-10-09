task default: :update do
  exec "jekyll serve --watch"
end

task :update do
  require 'yaml'
  donations = YAML.load_file("donations.yml")
  File.open("_includes/area.html", "w") do |f|
    donations.each do |donation|
      f.write <<-EOF
        <area onmouseover="d(this)" onmouseout="e(this)" shape="rect" coords="#{donation['rect']}" href="#{donation['href']}" title="#{donation['alt']}"/>
      EOF
    end
  end

  File.open("_includes/list.html", "w") do |f|
    donations.each do |donation|
      (x1, y1, x2, y2) = donation['rect'].split(",").map(&:to_i)
      amount = (y2 - y1) * (x2 - x1)

      f.write <<-EOF
        <li><a href="#{donation['href']}" alt="#{donation['alt']}">#{donation['donor']}</a> donated $#{amount}</li>
      EOF
    end
  end
end
