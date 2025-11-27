# Why to Use .gitignore File?

When you make commits in a git repository, you choose which files to stage and commit by using **git add FILENAME** and then **git commit**. But what if there are some files that you never want to commit? It's too easy to accidentally commit them (especially if you use **git add .** to stage all files in the current directory). That's where a **.gitignore** file comes in handy. It lets Git know that it should ignore certain files and not track them.

## What Kind of Files Should You Ignore?

- Log files
- Files with API keys/secrets, credentials, or sensitive information
- Useless system files like **.DS_Store** on macOS
- Generated files like **dist** folders
- Dependencies which can be downloaded from a package manager
- And there might be other reasons (maybe you make little [**todo.md**](http://todo.md/) files)