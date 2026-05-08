---
---
A Version Control System (VCS) is a system that records changes done on a file over time and allows you to recall an old version of that. It can be centralized or distributed.
The main goal of a VCS are: allows the developers to work simultaneously, without overwriting their changes; maintains a history of all changes.
## Git
Git is a VCS that maintains, in the Git Repository, that history of all type of file: who made the changes, when the changes ware made, what was changed and some notes and comments about the changes.
### State
A file in Git can have three status:
- Commit
	Means that the file is safely storage in your database.
- Modified
	Means that you have modified the file, but it doesn't committed yet. You are modifying the file.
- Staged
	Means that you finish modifying the file, and it goes into next commit snapshot.
### Section
In git a file can be in three section:
- Git directory
	Is the database that contains all file (Metadata and object) in git.
- Working directory
	Is the place where the files go when they are pulled out compressed database in git directory.
- Staging area
	Is the place, generally contained in the git directory, where there is the file that will go in the next commit.
When you want to modify a file the workflows is the something like this: the file from the git directory go into the working directory; one time that the changes are finished, the file goes into the staging area, and then it will be gone in the next commit.
## GitHub
GitHub is the hosting places that uses Git and where the files are storage. The container of a file is called repository.