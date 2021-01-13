# Linux / bash  

##  Questions

1. What is (i)`stdin`, (ii)`stdout`, (iii)`stderr`?
(i) 
- `stdin` is the standard input stream 
- accepts text as input 

(ii)
- `stdout` is the standard out stream 
- Error messages from command are sent through `stdout` 

- Streams are treated as though they were files 
- You can read text from a file, and into a file : both of these actions involve a stream of data 
- The concept of handling a stream of data as a file is quite natural 
- Each file associated with a process is allocated a unique number to identify it : the <b>file descriptor</b>
- The following values are used for (i)`stdin`, (ii)`stdout` and (iii) `stderr`: 
    - 0 : `stdin`
    - 1 : `stdout` 
    - 2 : `stderr` 

2. How do I pipe `stdout` to a file while reading `stderr`?
Because `stdout` and `stderr` are directed to different streams, we can pipe one while viewing the other. 
E.g. : 
If we make a file `err.sh`:
```
#!/bin/bash 

echo "About to try to access a file that doesn't exist" 
cat bad-filename.sh
```
Then run it `./err.sh`, both the `stdout` and `stderr` are printed. 
If we redirect the output however, `./err.sh > capture.txt`, then `cat capture.txt`, we can see that only 
the `stdout` has been piped/redirected. 
` > ` works with `stdout` by default, but specific streams can be specified: 
```
1 > # redirects stdout 
2 > # redirects stderr
```
We can try this with `./err.sh 2> capture_err.txt`. Now `stdout` is sent to the terminal and `stderr` is piped.
We can redirect `stdout` and `stderr` separately as: 
`./err.sh 1> capture.txt 2> capture_err.txt`
We could also redirect both to the same file as: 
`./err.sh > capture.txt 2>&1`
which uses the `&>` redirect instruction : tells the shell to make one stream go in the same direction as the other.
Here that means send stream 2 `stderr` to same destination as the default stream 1 `stdout`.

3. How do I print `stderr` from python?
The default in python is to write to `sys.stdout` whenever we use `print()`. 
We can redirect the output to `stderr` if desired - in principle this could be useful (i) to keep a log or (ii) to stop printing to `stdout` (?).

```
import sys 

print("Hello world", file=sys.stderr)
```

<b> I don't understand this. If I run this in a file, I still get a print statement in the screen?</b>

4. What are &, &&, ||, |, [, $, `set -e`, `set +e`, brace expansion in bash?
- & means run in the background e.g ./run_file & 
- && means AND. it runs both commands in parallel
- || means OR. it runs the command on the right only if the command on the left returned an error
- | is a pipe e.g. `cat dummy.txt | ./input.sh` takes `stdout` from `cat` and feeds it to `input.sh` 
- $ checks a variable's value 
- `[ ... ]` evaluates statements 
- <b>set +-e I don't know?</b> 

### parameter expansion 
- basic form of parameter expansion is `${param}$` where the value of `param` is substituted
- expansions for e.g. removing prefixes + suffixes 
- e.g. `file=data.txt`
- `${VAR%pattern}$` removes a matching suffix e.g. `${file%.*}$` returns `data`
- `${VAR#pattern}$` removes a matching prefix e.g. `${file#*.}$` returns `.txt`
- # comes before % on keyboard since prefixes come before suffixes

- so much additional possible complexity here and I find it rather confusing...!! 

5. What does `for x in {1..3}; do echo ${x%.xyz}; done` do?
It loops through an array [1,2,3] and prints out the result of each item in array removing the suffix .xyz

## Tools I use daily/weekly
### wget / curl
- `wget` is a tool to download files from servers 
- `curl` is a tool that lets you exchange requests/responses with a server 
- usually use them in the same way to DL files e.g. `wget url` or `curl url`

### sed
- make changes in a file 
- deep dark death 

- line oriented : default behaviour is change the first occurence on each line 

1. substitute
- substitute command `s` : changes first occurrences of expression in each line e.g. `sed s/old_match/new_match/ <old >new` changes old_match in old file to new_match in new file 
- equivalent `sed s/old_match/new_match/ old >new`

- or in the same file ` sed s/old_match/new_match <file`

- note old_match is a regular expression pattern search (wtv that means)

2. much more fancy stuff i dont understand 

### grep 
- searches given file for lines containing a match to given strings

1. search any line containing word in filename 
- `grep 'word' filename`

2. perform case-insensitive search 
- `grep -i 'word' filename`

3. look for all files in current dir + subdir for the word 'word'
- `grep -R 'word'`

4. search + display total ntimes that the string 'word' appears in a file `words.txt` 
- `grep -c 'word' words.txt`

5. more stuff 

### find 
- `find` takes a path to find things 
- `find /` finds + prints every file on the system 
- `find ~ -name '*.jpg'` search for files matching name "*.jpg" in home dir 

- apparently faster options 
- i just use find when i cant use ls 

### awk 
idk 

### tail/head

### touch 
generates file w name 

### which 

### wc 
counts  

## Tools I love

- tldr / cheat.sh
- https://github.com/junegunn/fzf
- tmux


## Advanced
- What is the \033]52;c; terminal escape sequence?
- Can you compile newest (stable) tmux from github? https://github.com/tmux/tmux
- How do you read stdin in Python? E.g. `find . -name "*.sdf" | python functionality.py > results.txt`
