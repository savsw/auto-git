Remembering to bump a version number with a commit is a tedious process.  At 
Savannah Scriptworks, our core development work is done in Node.JS.  The script
included herein automatically increments the version  of a project depending
upon the type of commit.  It determines what type of commit is occurring by
asking the developer one simple question.  In structuring the versioning process
we created the MAX versioning concept.

The script provided here is a supplemental library.  It runs the `git commit` 
process; it does not replace it.  If you ever run into a versioning dilemma
through branch merges or otherwise, you can always straighten things out
by manually editing the version tracking file or running a standard `git commit`.

MAX Versioning
==============
The idea behind MAX versioning is that every commit accomplishes one of three
possible tasks: a Milestone, feAture, or fiX.  MAX version numbers are three
integer values separated by dots.  example: 0.2.14  The first value indicates
the Milestone, the second the feAture and the final value represents the fiX.

A milestone is a point of total completion of a project.  It's a major release.

feAtures are large steps taken towards milestones.  These are the smaller points
of completion along the path towards the next major release.

Everything else is a fiX.  This includes bugfixes, housekeeping, tweaking, and
all other steps taken towards any point of completion.

Tracking the version number
===========================
It is natural for Node applications to include the file `package.json`.  Even if
the application is private and not published through npm, the package.json file
provides benefits such as streamlineing management of dependencies.

Because our preferred platform for development is Node.JS, we naturally track
the version number of our projects within the package.json file.  Forgetting to
increment the version and add the package.json file to commits is why we wrote
this script.

Install
=======
This is a bash script written for linux.  We assume it might work from a MAC, 
but it certainly won't work from windows.  We recommend copying the ssw_commit
file into the hooks directory of a git repository.  This can be found at 
```bash
#Run these commands from the root directory of your repository (after having run git init)
wget -O .git/hooks/ssw_commit http://github.com/savsw/auto-git/raw/master/ssw_commit
chmod 755 .git/hooks/ssw_commit
```
With the script copied to this location, we then need create an alias to make
it easier to call the script.  The goal of the script is to streamline
versioning during the commit process, so let's call the alias 'commit'
```bash
alias commit='$(git rev-parse --show-toplevel)/.git/hooks/ssw_commit || git commit'
```
Put this line above into your ~/.bash_aliases or ~/.bashrc file (depending on
which is proper for your environment).  After editing this file, you need to
either open a new terminal or source the file to make the changes active.
```bash
source ~/.bash_aliases
```
That's it!

Usage
=====
Now when you're working in a repository, instead of running `git commit` just
run `commit`.  Due to the alias we created, this will either run the MAX 
versioning script (if it exists in the hooks of the repository) or it'll
run the standard `git commit` command.

When the script runs, it'll ask the developer if the commit is a Milestone, 
feAture, or fiX and increment the version number accordingly.

License
=======
![LGPLv3](http://www.gnu.org/graphics/lgplv3-88x31.png)
