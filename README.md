Derryl's Command-Line Tools
===========================

Over time I've written some scripts and aliases that make my life a lot easier. I'm sharing them here, in case anyone else may find them useful.

Feel free to share your suggestions for improvement, etc.

###Summary
In my root user folder (~), I've piped my `.bash_profile` to `.bashrc` using:
```[ -r ~/.bashrc ] && source ~/.bashrc```

I then use my `.bashrc` to declare some aliases, simple functions, and point to external shell files for more complex functions (to prevent from slowing down my shell launch). Some examples:


##Git

####Clone a repo
	gcl() { git clone $1; }

####Commit changes quickly
	gcam() { git commit -am "$1"; }
	
####Basic commands
	alias gb='git branch'
	alias gpl='git pull'
	alias gps='git push'
	alias gdf='git diff'
	alias gst='git status && fact'
	alias glr='git pull --rebase && fact'
	gpsu() { git push --set-upstream origin '$1'; }
	
####Auto-completion of branch/tag names
	if [ -f ~/.git-completion.bash ]; then . ~/.git-completion.bash ; fi
	
####Detailed log of recent activity
	alias glog='git log --branches --date-order --graph --pretty=format:"%C(yellow)%h%C(cyan)%d %C(yellow)%ad%Creset %s%Creset %C(green)[%an]%Creset" --date=relative'
	
####Open repo website (if hosted on GitHub)
	function gh() {
	  giturl=$(git config --get remote.origin.url)
	  if [ "$giturl" == "" ]
	    then
	     echo "Not a git repository or no remote.origin.url set"
	     exit 1;
	  fi
	 
	  giturl=${giturl/git\@github\.com\:/https://github.com/}
	  giturl=${giturl/\.git//}
	  echo $giturl
	  open $giturl
	}


##Development

####Start a server

Serves the current dir on your port of choice, and opens it in your browser (e.g. `simple 8000`)

	simple()  { 
	    title "serving :$1"; 
	    open "http:///0.0.0.0:$1"; 
	    python -m SimpleHTTPServer $1; 
	}
	
####Upload a file to S3

I use [this script](https://github.com/mtigas/dotfiles/blob/master/bin/s3up.py) by Mike Tigas to perform the upload, to a dev bucket I've pre-configured.

	s3() {
	  if [ -n "$1" ]; then
	    python ~/.scripts/python/s3up.py $1;
	  else
	    echo 'must supply a filename to upload';
	  fi
	}
	
####Optimize an image (to reduce filesize)

Opens the file in [ImageOptim](http://imageoptim.com/), an awesome (and free) Mac program.

	optim() {
		open -a "ImageOptim" $1;
	}
	
####Convert a file to base-64

This one's pretty tight. Just type `b64 filename` and it's base-64 version will be copied to your clipboard.

	function b64() {
	  # openssl base64 -in $1 | tr -d "\n" | pbcopy;
	  cat $1 | base64 | pbcopy;
	}
	
	
####Open any file in Sublime Text 2	
	st() { open -a "Sublime Text 2" $1; }
	
####Open my .bashrc for editing
	alias rc="st ~/.bashrc"
	
	
##Vagrant

Pretty straightforward.

	alias vr="tfroot && vagrant reload"
	alias vd="tfroot && vagrant destroy"
	alias vu="tfroot && vagrant up"
	alias vs="tfroot && vagrant ssh && title 'SSH Vagrant'"


##Miscellaneous

####Open a file (or current dir)

Yes… I have a shorthand for this. Opens current dir in Finder if no argument is given.

	o() {
	  if [ ! -n "$1" ]; then
	    open .;
	  else
	    open $1;
	  fi
	}
	
####Make a directory and cd into it
	
	function mkcd {
	  if [ ! -n "$1" ]; then
	    echo "Enter a directory name"
	  elif [ -d $1 ]; then
	    echo "\`$1' already exists"
	  else
	    mkdir $1 && cd $1
	  fi
	}
	
####Add a title to shell window
	title() { 
		echo -ne "\033]0;$1\007";
	}

####Make a pretty-looking bash prompt

	CLICOLOR=1
	LSCOLORS=gxfxcxdxbxegedabagacad
	export PS1='\[\e[1;31m\]♫ \[\033[01;36m\] \w \[\033[00m\]'
	export TERM=xterm-256color

![image](http://powertothepeople.s3.amazonaws.com/terminal-prompt.png)