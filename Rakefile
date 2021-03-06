desc "Update from git and create symlinks"
task :update do
  update_from_git
  create_symlinks
  reload_zshrc
end

task :symlinks do
  create_symlinks
end

task default: 'update'

def all_files
  Dir.glob('.*')
end

def ignore_files
  ['.git', '.gitignore', '.', '..']
end

def update_from_git
  system('git pull')
end

def reload_zshrc
  system('source ~/.zshrc')
end

def create_symlinks
  dir = File.dirname(__FILE__)
  (all_files - ignore_files).each do |file|
    homedir = File.expand_path ENV['HOME']
    source_path = File.join dir, file
    target_path = File.join homedir, file
    if File.exists?(target_path)
      puts "File #{target_path} exists.  Overwrite it (y/n)?"
      if STDIN.gets.chomp.downcase == 'y'
        puts "Deleting #{target_path}..."
        raise "This shouldn't happen, but if it does, I'm refusing to delete /" if target_path == "/"
        FileUtils.rm_r(target_path)
      else
        puts "Skipping #{target_path}..."
        next
      end
    end

    puts "Symlink #{source_path} -> #{target_path}"
    FileUtils.ln_s(source_path, target_path)
  end

  puts
  puts "Done."
end
