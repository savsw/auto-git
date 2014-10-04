Remembering to bump a version number with a commit is a tedious process.  This 
project provides a commit script designed to automatically increment the version
of a project found in a `package.json` file (commonly used by Node.JS projects).
The script included herein automatically increments the version of a project 
depending upon the type of commit.  It determines what type of commit is
occurring by asking the developer one simple question.  In structuring the
versioning process we created the MAX versioning concept, which might not be
a unique concept, but we've taken the time to formalize it.

The script provided here is a supplemental library.  It runs the `git commit` 
process; it does not replace it.  If you ever run into a versioning dilemma
through branch merges or otherwise, you can always straighten things out
by manually editing the version tracking file or running a standard `git commit`.

MAX Versioning
==============
The idea behind MAX versioning is that every commit accomplishes one of three
possible tasks: a Milestone, feAture, or fiX.  MAX version numbers are three
integer values separated by dots.  Example: `0.2.14`.  The first value indicates
the Milestone, the second the feAture and the final value represents the fiX.

A milestone is a point of total completion of a project.  It's a major release.

feAtures are large steps taken towards milestones.  These are the smaller points
of completion along the path towards the next major release.

Everything else is a fiX.  This includes bugfixes, housekeeping, tweaking, and
all other steps taken towards any point of completion.

vs Semantic Versioning
======================
MAX versioning fills a different need from Semantic Versioning. If the project
is an API that other developers will rely on, it should use Semantic Versioning
to promote stability for users of the API. MAX versioning was created to provide
sensible versioning for smaller or more fluid projects.

Tracking the version number
===========================
It is natural for Node applications to include the file `package.json`.  Even if
the application is private and not published through npm, the package.json file
provides benefits such as streamlineing management of dependencies.

This script was designed for Node.JS projects and so it maintains the version
number of the project using the `package.json` file.  Forgetting to increment 
the version in this file and then add this file to the commit is why we wrote
this script.

Install
=======
This is a bash script written for linux.  We assume it might work from a mac, 
but it certainly won't work from windows.  We recommend copying the ssw_commit
file into the hooks directory of a git repository.  This can be done by:
```bash
#Run these commands from the root directory of your repository
#  - this can only be done after the repository is initialized
wget -O .git/hooks/ssw_commit http://github.com/savsw/auto-git/raw/master/ssw_commit
chmod 755 .git/hooks/ssw_commit
```

With the MAX versioning script installed to your repository, the next step is
to create an alias to make it easier to access. How you add the alias depends
on what shell you use. We provide examples for bash and fish below.

Don't Forget
============
There's just one last step.  Edit the ssw_commit script to match the needs of
the repository.  This might be as simple as editing the FILE declaration to
point to the relative location of your `package.json` file. Critically, the
script needs to know where to find the version number of the project.

BASH alias
==========
```bash
alias commit='$(git rev-parse --show-toplevel)/.git/hooks/ssw_commit || git commit'
```
Put this line above into your ~/.bash_aliases or ~/.bashrc file (depending on
which is proper for your environment).  After editing this file, you need to
either open a new terminal or source the file to make the changes active.
```bash
source ~/.bash_aliases
```
FISH function
=============
```fish
function commit
  eval (git rev-parse --show-toplevel)/.git/hooks/ssw_commit; or git commit
end
```
Put that line in your ```~/.config/fish/config.fish``` and open a new terminal.

Usage
=====
Now when you're working in a repository, instead of running `git commit` just
run `commit`.  Due to the alias we created, this will either run the MAX 
versioning script (if it exists in the hooks of the repository) or it'll
run the standard `git commit` command.

When the script runs, it'll ask the developer if the commit is a Milestone, 
feAture, or fiX and increment the version number accordingly.

Collaboration
=============
The commit script of this project recognizes one primary central authority that
enables collaboration.  The idea is that if all developers work against one
central authority, their version numbers won't collide.  If an origin is defined
for the repository, the script will fetch from origin before versioning and
push to origin after a successful commit.

During the commit process, the script will fetch from origin (if you prefer
to call the primary remote source something other than "origin", please edit the
ORIGIN  declaration in the script).  If there is a discrepancy between origin
and local, the commit will be aborted.  This comparison is done in attempt to
reduce discrepancies caused through the natural course of collaboration.  Race
conditions still exist, and if the version number should ever be corrupted as a
result, you can always manually edit the version number prior to a commit to put
things proper again.

License
=======
![LGPLv3](http://www.gnu.org/graphics/lgplv3-88x31.png)
