# Noteface
Noteface is a Sinatra application for managing the compilation and distribution of LaTeX documents that are updated in a Git repository (on GitHub) frequently.

Noteface compiles a PDF every time a change in a .tex file is committed to a particular repository of GitHub. It then allows others to download those PDFs, and it tracks downloads.

## Use case
I have a public repository of my [lecture notes](http://github.com/christhomson/lecture-notes), and it contained the .tex source files and the corresponding compiled .pdf files.

However, I wanted to stop committing PDF files to the repository for a number of reasons:

* Storing binary files that change constantly in Git seems a bit fugly since you can't easily perform a useful `diff` on them.
* If someone wants to make a correction, they currently have to correct it in the .tex source and either install LaTeX and compile the PDF (and I have to assume the PDF is a reasonable representation of the .tex), or just commit the .tex and I'll have to compile and commit the PDF separately.

Instead, Noteface is a web service that will perform all the compilations automatically. Noteface provides an endpoint that GitHub can hit through a [post-receive hook](https://help.github.com/articles/post-receive-hooks) which will trigger compiles of all the documents that were added or modified.

In addition, having some basic download statistics will be nice, and Noteface provides that. It stores some download stats in Redis and it also supports sending a download event to [Mixpanel](http://mixpanel.com).

Finally, since Noteface compiles the PDF files after a commit is made, we can include the commit's day/time and SHA in the PDF content. This is useful for people who download and save the PDF and want to know how outdated their version is.

## Dependencies
Noteface requires Ruby and a number of RubyGems which are defined in the `Gemfile`. It also requires Redis for keeping track of documents and downloads. And of course, it requires LaTeX to be installed (specifically, `pdflatex`). I recommend [installing LaTeX from source](http://www.tug.org/texlive/quickinstall.html).