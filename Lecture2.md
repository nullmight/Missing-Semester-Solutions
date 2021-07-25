# Lecture 2: Shell Tools and Scripting

1. Read `man ls` and write an `ls` command that lists files in the following manner
   * Includes all files, including hidden files
   * Sizes are listed in human readable format (e.g. 454M instead of 454279954)
   * Files are ordered by recency
   * Output is colorized

   A sample output would look like this
   ```bash
    -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
    drwxr-xr-x   5 user group  160 Jan 14 09:53 .
    -rw-r--r--   1 user group  514 Jan 14 06:42 bar
    -rw-r--r--   1 user group 106M Jan 13 12:12 foo
    drwx------+ 47 user group 1.5K Jan 12 18:08 ..
   ```
    #### **Solution:**
    
    ```bash
    ls -a -h -t -color
    ```
    
    Names of hidden files start with `.` and `-a` is used to display all files without ignoring those whose names
    start with .
    - h is used to display sizes in human readable format
    - t is used to sort by modification time, newest first
    - color is used to colorize the output
2. Write bash functions `marco` and `polo` that do the following. Whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.
   #### **Solution:**
   Prerequisites: Read up on bash functions, editing files in vim and `source` command
   1. Create a bash script named `marco.sh` and create a bash function named `marco` inside it, which when called stores the present working directory in a variable.

        ```bash
        vim marco.sh
        ```
        (Press `I` to enter insert mode)
        ```bash
        #!/usr/bin/env bash
        function marco()
        {
            var=”PWD”
        }
        ```
        (Press `Esc` to exit insert mode, then enter `:wq` to save and exit.)

   2. Create another bash script named `polo.sh` and create a bash function named `polo` inside it,
   which when called `cd`s into the path stored in the variable created by `marco`.
   
        ```bash
        vim marco.sh
        ```

        (Press `I` to enter insert mode)

        ```bash   
        #!/usr/bin/env bash
        function polo()
        {
            cd “var”
        }
        ```
        (Press `Esc` to exit insert mode, then enter `:wq` to save and exit.)

   1. Edit permissions of `marco.sh` and `polo.sh` and then load them into the shell using `source`
        ```bash
        chmod a+rx marco.sh polo.sh
        source marco.sh
        source polo.sh
        ```
3. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.
   ```bash
    #!/usr/bin/env bash
    n=$(( RANDOM % 100 ))

    if [[ n -eq 42 ]]; then
        echo "Something went wrong"
        >&2 echo "The error was using magic numbers"
        exit 1
    fi

    echo "Everything went according to plan"
   ```
   #### **Solution:**
   1. Create a bash script named `test.sh` and save the script given in the question inside it.
        ```bash
        vim test.sh
        ```
        (Press `I` to enter insert mode)

        ```bash
        #!/usr/bin/env bash
        n=$(( RANDOM % 100 ))
        if [[ n -eq 42 ]]; then
            echo “Something went wrong”
            >&2 echo “The error was using magic numbers”
            exit 1
        fi
        echo “Everything went according to plan”
        ```

        (Press `Esc` to exit insert mode, then enter `:wq` to save and exit.)

    1. Create another bash script named `errcnt.sh`. Inside it, create a variable named `cnt` to store
    count and two text files to store output and error of each run. Increment `cnt` by `1`. Run
    `test.sh` once and store the output and error inside their respective files. Then create a `while` loop that runs till the exit code of the last run of `test.sh` is equal to `0` and increments `cnt` by `1` and updates `output.txt` and `error.txt` in each iteration. This effectively simulates a `do while` loop. In the end, print the value of `cnt` and contents of `output.txt` and `error.txt`.

        ```bash
        vim errcnt.sh
        ```
        (Press `I` to enter insert mode)
    
        ```bash
        #!/usr/bin/env bash
        cnt=0
        touch output.txt
        touch error.txt
        ((cnt++))
        bash test.sh 1>> output.txt 2>> error.txt
        while [[ $? -eq 0 ]]
        do
            ((cnt++))
            bash test.sh 1>> output.txt 2>> error.txt
        done
        echo "Count is $cnt"
        cat output.txt
        cat error.txt
        ```

        (Press `Esc` to exit insert mode, then enter `:wq` to save and exit.)
    
4. As we covered in the lecture `find`’s `-exec` can be very powerful for performing operations over the files we are searching for. However, what if we want to do something with all the files, like creating a zip file? As you have seen so far commands will take input from both arguments and STDIN. When piping commands, we are connecting STDOUT to STDIN, but some commands like `tar` take inputs from arguments. To bridge this disconnect there’s the `xargs` command which will execute a command using STDIN as arguments. For example `ls | xargs rm` will delete the files in the current directory.

    Your task is to write a command that recursively finds all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces (hint: check `-d` flag for `xargs`).

    If you’re on macOS, note that the default BSD `find` is different from the one included in GNU coreutils. You can use `-print0` on `find` and the `-0` flag on `xargs`. As a macOS user, you should be aware that command-line utilities shipped with macOS may differ from the GNU counterparts; you can install the GNU versions if you like by using brew.

    #### **Solution:**
    ```bash
    find . -name '*.html' -print | zip allhtml.zip -@
    ```
    * `find` is used to recursively finds all HTML files(since they end with `.html`, `-name` option can be used to search them) in the current directory(`.`)
    
    * `-print` option prints all the HTML files and `|` is used to redirect them to `zip`
    
    * `zip` creates a new zip file named `allhtml.zip` containing all files from `find`

    * `-@ flag` is used to take list of files from standard input (here, redirected from `find` using `|`) and read one file name per line to ensure file names with spaces are read properly

5. Write a command or script to recursively find the most recently modified file in a directory. More generally, can you list all files by recency?
    #### **Solution:**
    ```bash
    find -printf "%TY-%Tm-%Td %TT %p\n" | sort -n
    ```
    `find` is used along with `-printf` option to format the output to show the last modified date and time followed by file names, `|` redirects the output to `sort -n` which sorts the list by numerical value



