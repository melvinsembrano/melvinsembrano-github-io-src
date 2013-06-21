guard 'coffeescript', :input => 'app/js', :output => 'public/js'
guard 'sass', :input => 'app/css', :output => 'public/css'
guard 'less', :output => 'public/css'
guard 'staticise', :input => 'app', :output => 'public'


def sync(src, dest)
  if File.exist?(src)
    FileUtils.mkdir_p(File.dirname(dest)) unless File.exist?(File.dirname(dest))
    FileUtils.cp(src, dest)
  else
    FileUtils.rm(dest) if File.exist?(dest)
  end
end


guard :shell, :all_on_start => false do
  watch(%r{^app/js/(.+\.coffee)$}) do |m|
    Dir.glob(File.join(Dir.pwd, "app/**/*.coffee-con")).each do |f|
      FileUtils.touch f
    end
  end
end

guard :shell, :all_on_start => true do

  watch(%r{^app/js/(.+\.js)$}) do |m|
    f = m.first
    dest = f.gsub(/app\/js/, "public/js")
    sync(f, dest)
  end

  watch(%r{^app/js/(.+\.coffee-con)$}) do |m|
    puts "compiling #{m.first}..."
    f = m.first
    dest = f.gsub(/app\/js/, "public/js")

    files = []
    File.readlines(f).each do |line|
      files << File.join("app", "js", line.strip.split("//= ").last) if line.start_with?("\/\/=")
    end

    `coffee --join #{ File.join(Dir.pwd, File.dirname(dest), "#{ File.basename(dest, '.coffee-con') }.js") } -c #{ files.join(" ") }` if files.length > 0
    true
  end

  watch(%r{^app/css/(.+\.css)$}) do |m|
    f = m.first
    dest = f.gsub(/app\/css/, "public/css")
    sync(f, dest)
  end

  watch(%r{^app/img/(.+\.(png|jpg|jpeg|ico|gif))$}) do |m|
    f = m.first
    dest = f.gsub(/app\/img/, "public/img")
    sync(f, dest)
  end

end
