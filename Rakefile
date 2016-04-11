require 'rake'

desc "install the dot files into user's home directory"
task :default do
  Dir['*'].each do |file|
    next if %w[README.md Rakefile].include? file
    delete_file(file) 
    link_file(file)
  end
  request_sudo
  install_homebrew
  install_vim
  install_vim_plugins
  install_chruby
  install_mri
  use_zsh
  make_tmp
end

def request_sudo
  `sudo cat /etc/shells`
end

def delete_file(file)
  puts "removing ~/.#{file}"
  `rm -rf "$HOME/.#{file}"`
end

def link_file(file)
  puts "linking ~/.dotfiles/#{file} as ~/.#{file}"
  `ln -s "$PWD/#{file}" "$HOME/.#{file}"`
end

def make_tmp
  `mkdir ~/.tmp`
end

def use_zsh
  print 'switching shells to zsh'
  `brew install zsh`
  `sudo echo $(which zsh) >> /etc/shells`
  `sudo chsh -s $(which zsh)`
end

def install_homebrew
  print "installing vim via homebrew"
  print ' (skipping)\n' && return if `which brew` =~ /\/usr\/local/
  `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
end

def install_chruby
  print "installing chruby and ruby-install via homebrew"
  print ' (skipping)\n' && return if `ls ` =~ /\/chruby\/chruby/
  `brew install chruby && brew install ruby-install`
end

def install_mri
  print "install Ruby (MRI) ? (y/n)"
  if gets.chomp == "y"
    puts "Instaling ruby, this isn't the fastest thing in the world."
    `ruby-install ruby`
  else
    print " (skipping)\n"
  end
end

def install_vim
  return if `which vim` =~ /\/usr\/local/
  puts "installing vim via homebrew"
  `brew install vim`
end

def install_vim_plugins
  if `ls ~/.vim/bundle/Vundle.vim` == ""
    puts 'cloning vundle'
    `git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
  end
  puts 'installing vundles, it might take a bit'
  `vim +PluginInstall +qall`
end
