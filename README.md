Derryl's Command-Line Tools
===========================

Over time I've written some scripts and aliases that make my life a lot easier. I'm sharing them here, in case anyone else may find them useful.

Feel free to share your suggestions for improvement, etc.

###Summary
In my root user folder (~), I've piped my `.bash_profile` to `.bashrc` using:
```[ -r ~/.bashrc ] && source ~/.bashrc```

I then use my `.bashrc` to declare some aliases, simple functions, and point to external shell files for more complex functions (to prevent from slowing down my shell launch). Some examples:

- **mkcd** - makes a directory and jumps to it (instead of `mkdir x && cd x`)
- **clone *dir url*** - clones a repo to my desired destination
- **run *project_name*** - begins serving one of my web projects
- **getnew *repo_name*** - pulls the latest for a specified repo (defaults to a couple of my in-progress projects)
- and so forth…