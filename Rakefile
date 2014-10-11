task default: :update do
  exec "jekyll serve --watch"
end

task :update do
  require 'yaml'
  donations = YAML.load_file("donations.yml")
  File.open("_includes/area.html", "w") do |f|
    donations.each do |donation|
      donation['images'].each do |(_, coords)|
        f.write <<-EOF
          <area onmouseover="d(this)" onmouseout="e(this)" shape="rect" coords="#{coords}" href="#{donation['href']}" title="#{donation['alt']}"/>
        EOF
      end
    end
  end

  total = 0
  File.open("_includes/list.html", "w") do |f|
    donations.each do |donation|
      amount = 0
      donation['images'].each do |(_, coords)|
        (x1, y1, x2, y2) = coords.split(",").map(&:to_i)
        amount += (y2 - y1) * (x2 - x1)
      end
      total += amount

      f.write <<-EOF
        <li><a href="#{donation['href']}" alt="#{donation['alt']}">#{donation['donor']}</a> donated $#{amount}</li>
      EOF
    end
  end

  File.open("_includes/stats.html", "w") do |f|
    f.write <<EOF
<div id="stats">
  <font id="stat1">Donated:</font> <font id="statgreen">$#{total}</font>
  <br>
  <font id="stat1">Available:</font> <font id="statgreen">#{1_000_000 - total}</font>
</div>
EOF
  end

  pages = donations.map { |d| d['images'] }.flatten.map do |images|
    images.map do |(name, coords)|
      (x1, y1, _, _) = coords.split(",")
      "+#{x1}+#{y1} pixels/#{name}"
    end
  end.flatten
  sh "convert -page 1000x1000#{pages.join(" -page ")} -background none -compose DstOver -flatten image-map.png"
end
