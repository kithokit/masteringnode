
# Installing Node

In this chapter we will be looking at the installation and compilation of node. Although there are several ways we may install node, we will be looking at [homebrew](http://github.com/mxcl/homebrew), and the most flexible method, of course - compiling from source.

### Homebrew

Homebrew is a package management system for _OSX_ written in Ruby, is extremely well adopted, and easy to use.  Homebrew can be installed in a number of ways.  Possibly the easiest way is to perform a quick install to `/usr/local/`:

	$ ruby -e "$(curl -fsSL https://gist.github.com/raw/323731/install_homebrew.rb)"

Next, to install node via the `brew` executable, simply run:

	$ brew install node.js

For more information on packages and commands available to homebrew, checkout the README at github.

## Building From Source

To build and install node from source, we first need to obtain the code. The first method of doing so is
via `git`, if you have git installed you can execute:

    $ git clone http://github.com/joyent/node.git && cd node
    $ git checkout v0.4.0

For those without _git_, or who prefer not to use it, we can also download the source via _curl_, _wget_, or similar:

    $ curl -# http://nodejs.org/dist/node-v0.4.0.tar.gz > node.tar.gz
    $ tar -zxf node.tar.gz

Now that we have the source on our machine, we can run `./configure` which discovers which libraries are available for node to utilize such as _OpenSSL_ for transport security support, C and C++ compilers, etc. `make` which builds node, and finally `make install` which will install node.

    $ ./configure && make && sudo make install

## Installing from Distribution Sources

Installing node.js from a distribution's repositories is not highly recommended.  This is because the version included with your distribution may be very outdated.  As an example, the original version of this document was written to target node.js 0.1.99.  In Ubuntu 10.10, the version of node.js included in the official repository is 0.1.97-1build1.  Because node.js is a relatively young project with a large community, changes to the API occur quickly and often.

If you still wish to install nodejs from an official repository, it can be done in the usual way:

    $ sudo apt-get install nodejs
    $ yum install nodejs

Refer to your distribution's documention or wiki for the proper commannd for installation from the official repository.

### npm: Node Package Manager

npm is a node package manager with a relatively large category of available modules.  To install directly in one line, run:

	$ curl http://npmjs.org/install.sh | sh

If this one-liner fails, run:

	$ git clone http://github.com/isaacs/npm.git
	$ cd npm
	$ sudo make install

For more information on npm, check out the [repo](https://github.com/isaacs/npm).

## Installing other packages

Using npm, we'll install other packages needed for the examples in this book.

	$ sudo npm install connect express nodeunit geddy zombie node-inspector

Let npm do it's thing.  It will install these modules any any dependencies.  You'll see output similar to the following:

	npm info activate jsdom@0.1.23
	npm info postactivate jsdom@0.1.23
	npm info build Success: express@1.0.7
	npm info build Success: nodeunit@0.5.1
	npm info build Success: mime@1.2.1
	npm info build Success: geddy@0.1.3
	npm info build Success: websocket-client@1.0.0
	npm info build Success: connect@1.0.1
	
## Setting up an editor

Part of setting up a development environment is using the right tools.  While some may prefer a full IDE, others will go with a very simple text editor.  I enjoy using *vim*, and this is my *~/.vimrc* file's contents:

    syntax on " enabled syntax highlighting
    :set number " line numbers
    :set ai " autoindent
    :set tabstop=4 " sets tabs to 4 characters
    :set shiftwidth=4
    :set expandtab
    :set softtabstop=4 " makes the spaces feel like real tabs
    
    " CSS (tabs = 2, lines = 79)
    autocmd FileType css set omnifunc=csscomplete#CompleteCSS
    autocmd FileType css set sw=2
    autocmd FileType css set ts=2
    autocmd FileType css set sts=2
    
    " JavaScript (tabs = 4, lines = 79)
    autocmd FileType javascript set omnifunc=javascriptcomplete#CompleteJS
    autocmd FileType javascript set sw=4
    autocmd FileType javascript set ts=4
    autocmd FileType javascript set sts=4
    " autocmd FileType javascript set tw=79

    " Jade
    autocmd FileType jade set omnifunc=jadecomplete#CompleteJade
    autocmd FileType jade set sw=2
    autocmd FileType jade set ts=2
    autocmd FileType jade set sts=2

    " ejs
    autocmd FileType ejs set sw=2
    autocmd FileType ejs set ts=2
    autocmd FileType ejs set sts=2

    " Highlight current line only in insert mode
    autocmd InsertLeave * set nocursorline
    autocmd InsertEnter * set cursorline

    " Makefiles require TAB instead of whitespace
    autocmd FileType make setlocal noexpandtab

    " Highlight cursor
    highlight CursorLine ctermbg=8 cterm=NONE
    
If your install of *vim* doesn't include the _*complete_ functions, you can download them from [vim.org/scripts](http://www.vim.org/scripts/index.php).  Most likely, you'll need the [jade.vim](http://www.vim.org/scripts/script.php?script_id=3192) script.

Other excellent IDEs include:

 1. [cloud9 IDE](http://cloud9ide.com/)
 2. [geany](http://www.geany.org/)
 3. [Aptana Studio](http://aptana.org/)
