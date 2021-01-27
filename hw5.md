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


5. What does `for x in {1..3}; do echo ${x%.xyz}; done` do?
It loops through an array [1,2,3] and prints out the result of each item in array removing the suffix .xyz

## Tools I use daily/weekly
### wget / curl
- `wget` is a tool to download files from servers 
- `curl` is a tool that lets you exchange requests/responses with a server 
- usually use them in the same way to DL files e.g. `wget url` or `curl url`

### sed
- sed is short for stream editor 
- USE THIS TO SEARCH + REPLACE 
- edits operations on text coming from stdin or file 
- edits line-by-line and in a non-interactive way 
- note that the macos version is different but can be installed with homebrew using `brew install gnu-sed`

- default behaviour is to perform edit on <b>first occurrence</b> of a line
- also default behaviour to output everything to stdout unless redirected (will just print to screen)

- Basic usage: 
``` 
sed [options] commands [file-to-edit]
```

#### Simple example: 
- `sed '' file` will just print file contents to screen 

- You can also easily pipe output. 

- `sed 'p' file` uses the explicit print `p` command. This will print each line twice (line by line). This illustrates line-by-line character 

- `sed -n 'p' file` suppresses automatic printing and uses explicit printing

#### Specifying parts of text stream 
- `sed -n '1p' file` prints the first line of the file (the 1 is the line number) 
- `sed -n '1,5p file` prints lines 1-5 (a range) 
- `sed -n '1,+4p' file` also prints lines 1-5 
- `sed -n '1~2p' file` prints every other line starting at 1 

#### Deleting text 
- Let's remove `-n` because it's useful to see what is left (not deleted) 
- `sed '1~2d' file` deletes every other line starting at 1 (<b>d instead of p</b>) and prints the rest 
- Remember at any point we can redirect output to a file `sed 1~2d file > everyother.txt`
- We can also edit inplace using `-i` but be careful because this does overwrite the original file 
- `sed -i '1~2d' everyother.txt` edits in place 
- we can also make a backup with `sed -i.bak '1~2d' everyother.txt` which creates a backup with `.bak` extension and edits original file in-place

#### Search and replace 
- `sed` is most well-known for substituting text 
- you can search for text patterns using regular expressions and replace text with something else 
- In its most basic form, you can change one word to another with the syntax `'s/old_word/new_word/'`
- Note you can use a different delimiter such as _ 
- Here we can print a url and modify it with sed, using the _ as delimiter: 
```
echo "http://www.example.com/index.html" | sed 's_com/index_org/home_'
```
which replaces `com/index` with `org/home`: 
`http://www.example.org/home.html`
- Note you need the fnal delimiter or sed will complain about unterminated commands.

to continue from this page https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux 

### grep 
- searches given file for lines containing a match to given strings
- or antipatterns
- can also grep in folder 

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
- `find ~ -name '*.jpg'` search for files matching name `*.jpg` in home dir 

- doesn't store in memory
- i just use find when i cant use ls 
- there are faster tools

### parallel
- parallel across processors 

### awk 
- awk is a scripting language for manipulating data 
- Useful when there are columns / column-based operations

It can:
- scan a file line by line 
- split each input line into fields
- compare input line/field to pattern 
- perform action(s) on matched lines 

Useful for: 
- transforming data files 

Syntax:
```
awk options 'selection_criteria {action }' input_file > output_file
```

e.g. 
```
$cat > employee.txt

ajay manager account 45000
sunil clerk account 25000
varun manager sales 50000
amit manager account 47000
tarun peon sales 15000
deepak clerk sales 23000
sunil peon sales 13000
satvik director purchase 80000 
```

1. `awk '{print}' employee.txt`
prints every line of data (no pattern given)

2. `awk '/manager/ {print}' employee.txt`
prints only lines containing pattern 
```
ajay manager account 45000
varun manager sales 50000
amit manager account 47000 
```

3. `awk '{print $1,$3}' employee.txt`
splits by whitespace character and stores in $n. if a line has 4 words, it's stored in $1 to $4. $0 prints the whole line.
```
ajay 45000
sunil 25000
varun 50000
amit 47000
tarun 15000
deepak 23000
sunil 13000
satvik 80000 
```

Built in varaibles in awk: 
- `NR` : keeps count of # of input records (lines)
e.g. `awk '{print NR, $0}' employee.txt` outputs:
```
1 ajay manager account 45000
2 sunil clerk account 25000
3 varun manager sales 50000
4 amit manager account 47000
5 tarun peon sales 15000
6 deepak clerk sales 23000
7 sunil peon sales 13000
8 satvik director purchase 80000 
```
prints the line number and then the line 

or `awk 'NR==3, NR==6 {print NR, $0}' employee.txt` outputs: 
```
3 varun manager sales 50000
4 amit manager account 47000
5 tarun peon sales 15000
6 deepak clerk sales 23000 
```
displays lines 3 to 6, with the line number and the line

- `NF` keeps count of the number of field in the current input record (line) 
e.g. `awk '{print $1, $NF}' employee.txt` outputs: 
```
ajay 45000
sunil 25000
varun 50000
amit 47000
tarun 15000
deepak 23000
sunil 13000
satvik 80000 
```
ie. it prints the last field with $NF

- `FS` contains field separator character (usually white space) - this can be reassigned 

- `RS` stores the current record separator (newline) 

- `OFS` stores output separator (field when awk prints them) - normally white space 

- `ORS` separates output lines - newline character 


### tail/head
- can print file until something

### touch 
- generates file w name 
- updates timestamp 
- looking for changed files 

### which 
- look for python version/versions of anything 
- vim `which tldr`

### wc 
ls | wc -l 
counts  

## Tools I love
- tldr / cheat.sh (MUCH better than man)
- ctrl + r then typing eg pip shows you the last pip commands executed
- https://github.com/junegunn/fzf : ctrl + r upgrade
- tmux : can have separate sessions + keeps sessions alive, resize terminals
    - change shortcuts 
    - look at jimmys dotfiles 

## Example 
- download csv file eg chembl 
- swap two columns in csv file with awk 
- select certain rows with grep 
- replace parts of name with sed 
- cat filename.csv | awk | grep | sed > filename2.csv

## Advanced
1. What is the \033]52;c; terminal escape sequence?
- see https://gist.github.com/fnky/458719343aabd01cfb17a3a4f7296797 for useful help
- escape sequences in general are a combination of characters that together have a meaning other than the literal characters therein 
- marked with one or more and possibly terminating characters 
- eg in C an escape sequence starts with a backslash \ 

- ANSI escape sequences are a sequence of ASCII characters with the following prefixes: 
    - ctrl key `^[`
    - octal `\033`
    - unicode `\u001b`
    - hexadecimal `\x1b`
    - decimal `27`

    followed by command, usually delimited by opening sq bracket `[` and followed by arguments and command

- e.g. `\x1b[1;31m` sets style to bold with red foreground 
- `ESC[{code};{string};{...}p` redefines a keyboard key to a specified string 

- Here the `\033]` is the octal escape sequence + delimiter 
- 52 is the code for the left arrow 
- should replace left arrow with c key 
- trying this in terminal doesn't work! 

2. Can you compile newest (stable) tmux from github? https://github.com/tmux/tmux
On mac this was the following steps: 
- `git clone https://github.com/tmux/tmux`
- `pip install autoconf`
- `brew install automake`
- `brew install pkg-config` 
- `brew install ncurses`
- `cd tmux`
- `sh autogen.sh`
- `./configure && make`

Can run tmux like `tmux` but can do a lot of customisation

On cluster there is a lot of nastyness :
```
TMUX_VERSION=2.3
INSTALL_DIR=/group/hepheno/smsharma

# create our directories
mkdir -p $INSTALL_DIR/local $INSTALL_DIR/tmux_tmp
cd $INSTALL_DIR/tmux_tmp

# download source files for tmux, libevent, and ncurses
wget -O tmux-${TMUX_VERSION}.tar.gz https://github.com/tmux/tmux/releases/download/${TMUX_VERSION}/tmux-${TMUX_VERSION}.tar.gz
wget https://github.com/downloads/libevent/libevent/libevent-2.0.19-stable.tar.gz
wget ftp://ftp.gnu.org/gnu/ncurses/ncurses-5.9.tar.gz

# extract files, configure, and compile

############
# libevent #
############
tar xvzf libevent-2.0.19-stable.tar.gz
cd libevent-2.0.19-stable
./configure --prefix=$INSTALL_DIR/local --disable-shared
make
make install
cd ..

############
# ncurses  #
############
tar xvzf ncurses-5.9.tar.gz
cd ncurses-5.9
./configure --prefix=$INSTALL_DIR/local 
make
make install
cd ..

############
# tmux     #
############
tar xvzf tmux-${TMUX_VERSION}.tar.gz
cd tmux-${TMUX_VERSION}
./configure CFLAGS="-I$INSTALL_DIR/local/include -I$INSTALL_DIR/local/include/ncurses" LDFLAGS="-L$INSTALL_DIR/local/lib -L$INSTALL_DIR/local/include/ncurses -L$INSTALL_DIR/local/include"
CPPFLAGS="-I$INSTALL_DIR/local/include -I$INSTALL_DIR/local/include/ncurses" LDFLAGS="-static -L$INSTALL_DIR/local/include -L$INSTALL_DIR/local/include/ncurses -L$INSTALL_DIR/local/lib" make
cp tmux $INSTALL_DIR/local/bin
cd ..

# cleanup
rm -rf $INSTALL_DIR/tmux_tmp
```
Τhen in bashrc : 
`alias tmux=$INSTALL_DIR/local/bin/tmux`

3. How do you read stdin in Python? E.g. `find . -name "*.sdf" | python functionality.py > results.txt`
If we have eg a bunch of sdf files, the following python file would read in each file from stdin and 
do something with it : 
```
import sys 

# do not read into list but rather use a generator which yields the lines as they're requested  
for name in sys.stdin:
    name = name.strip()
    print("name", name)

    # do something with file....
    with open(name, "r") as f: 
        lines = f.readlines()
    lines = [line.strip() for line in lines]

    print("file contains", lines)
```


