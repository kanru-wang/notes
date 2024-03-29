# Git

- git log --oneline (view simple log)
- git clean -f (delete all untracked files. "git clean -n" peeks files to be deleted before running "git clean -f")
- git reset --hard (remove all staged changes)
- git add -f filename (add a file of a gitignored file type)
- git checkout branch_name file_name (How to get just one file from another branch? https://stackoverflow.com/questions/2364147 Need to be in the correct folder and use the full file path)


# Shell

- `q` (quit "git diff")
- (working with VIM https://stackoverflow.com/questions/11828270/how-do-i-exit-the-vim-editor)
- (ways to pass the answer to a shell script promopt question in one line: https://stackoverflow.com/questions/3804577/have-bash-script-answer-interactive-prompts)
- `ls -lh` (check file sizes)
- `du -sh *` (check file sizes)
- `du -sh */*` (check file sizes one level down)
- `gzip -dr path_name/` (recursively unzip)
- (tar and un-tar a file https://askubuntu.com/questions/25347)
- (try `git fetch`, if see this message "Your branch is ahead of 'origin/develop' by xx commits.")
- `top` (check memory usage)
- (use `grep` to search and usage of flags https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)
- `grep 'some_keywords' */result/*/*` (for searching the "some_keywords" in the wanted files in the following file structure.)
    - aaa/result/8am/events_0
    - aaa/result/8am/events_1
    - aaa/result/9am/events_0
    - bbb/result/8am/events_0
    - bbb/result/8am/events_1
    - bbb/not_result/9am/log
    - bbb/result/9am/events_0
- `sed 's/word_to_search/word_to_replace/g' file_to_search.txt` (search a word and replace all occurrences)
- `cut` (select part of the text)
- `wc -l file_path` (count the number of lines in that file)
- `sort namelist.txt | uniq -c` (find the unique and count the duplicates; uniq requires doing sort first)
- `ls folder_name` (list what's in a folder)
- `cat file.txt | jq` (pretty-printing a JSON document, https://earthly.dev/blog/jq-select/)
- `>` (output and overwrite a file)
- `>>` (output and append to a file)
- Ctrl + A: Move cursor to the beginning of the line.
- Ctrl + E: Move cursor to the end of the line.
- Use `Ctrl + c` or `kill -9 %[enter_job_number]` to terminate a program, whereas `Ctrl + z` just put a program into sleep. Use `jobs` to check existing jobs in the background and their job numbers.
- `find /your/folder/path/ -size 0 -exec rm {} +` (Removes all zero-byte files from the filesystem)
- `find /your/folder/path/ -type f ! -name "*.jpg" -exec rm {} +` (Removes any file that does not have a .jpg extension)
 
