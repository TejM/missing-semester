# Solutions - Lesson 1

1. For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don’t have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) or a Linux virtual machine to use Unix-style command-line tools. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. If it says something like `/bin/bash` or `/usr/bin/zsh`, that means you’re running the right program.
      ```sh
       echo $SHELL
      ```

2. Create a new directory called `missing` under `/tmp`.
      ```sh
       cd tmp
       mkdir missing
      ```
      or
      ```sh
       mkdir tmp/missing
      ```

3. Look up the touch program. The man program is your friend.
      ```sh
        man touch
      ```

4. Use touch to create a new file called semester in missing.
      ```sh
        touch semester
      ```

5. Write the following into that file, one line at a time:
    ```sh
      #!/bin/sh
      curl --head --silent https://missing.csail.mit.edu
    ```
    The first line might be tricky to get working. It’s helpful to know that `#` starts a comment in Bash, and `!` has a special meaning even within double-quoted (`"`) strings. Bash treats single-quoted strings (`'`) differently: they will do the trick in this case. See the Bash [quoting manual](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) page for more information.
      ```sh
        echo '#!/bin/sh' >> semester
        echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
      ```

6. Try to execute the file, i.e. type the path to the script (`./semester`) into your shell and press enter. Understand why it doesn’t work by consulting the output of `ls` (hint: look at the permission bits of the file).
      ```sh
        ./semester
      ```
      ```console
        -bash: ./semster: No such file or directory
      ```
    **Answer:** The file permissions for `semester` are `-rw-r--r--` which means that this file is not executable. So trying to executing using `./` doesn't work.

7. Run the command by explicitly starting the `sh` interpreter, and giving it the file `semester` as the first argument, i.e. `sh semester`. Why does this work, while `./semester` didn’t?

    **Answer:** `./semester` makes your shell run the file as if it was a regular executable, i.e. it requires both execution and readable bits. Where as for `sh semester` command, the terminal(bash) will create a shell process that interprets the contents of `semester` as an argument, i.e. sh only requires the read permission for the file.

8. Look up the `chmod` program (e.g. use `man chmod`).

9. Use `chmod` to make it possible to run the command `./semester` rather than having to type `sh semester`. How does your shell know that the file is supposed to be interpreted using sh? See this page on the [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line for more information.

    **Answer:** You can change the file permissions using:
      ```sh
        chmod +x semester
      ```
      This will make the file into the executable. Secondly, the first character sequence(the shebang) in `sememster` reads `#!/bin/sh`. This tells the program loader to execute the file using the [Bourne shell(`sh`)](https://en.wikipedia.org/wiki/Bourne_shell)
      , or a compatible shell, located in the `/bin` directory.

9. Use `|` and `>` to write the *“last modified”* date output by `semester` into a file called `last-modified.txt` in your home directory.

    **Answer:** Using `man date` shows us that you can use the `-r` flag on data to get the last modified date for a file. We need to pipe the output of that to a file. However something like `date -r semester | echo > last-modified.txt` won't work as the `echo` command doesn't accept anything from stdin. For that we can use `cat`.

      ```sh
        date -r semester | cat > last-modified.txt
      ```




