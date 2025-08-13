# Shell Scripting Tutorial

By Muhammad Waleed Akram

* * *

## Introduction

Shell scripting is the art of telling efficiently your operating system (OS)
exactly what to do. This tutorial aims to take you from basic shell scripting
to a level where you can comfortably automate complex tasks and work like a
real system engineer.

## What Exactly is a Shell?

A **shell** is an interface between you and the kernel. It takes commands you
type and executes them on your behalf.

Some popular shells are:

  * **Bash** (Bourne Again Shell)
  * **Zsh**
  * **Fish**

Shell scripting is simply writing multiple shell commands into a file so that
they can run sequentially.

## 1\. Setting Up

Ensure you have access to a shell environment:

  * Linux/macOS: Open the Terminal
  * Windows: Use Git Bash or WSL (Windows Subsystem for Linux). I am using Ubuntu because RISC-V tools work well with it.

    
    
    echo $SHELL

![checking shell](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/1.png)

## 2\. Your First Script

Create a file named `myscript.sh`:

    
    
    vim myscript.sh

Then type:

    
    
    #!/bin/bash
    echo "Hello, Shell World!"

Save and exit with `:wq`.

![vim](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/2.png)

### Understanding the Script Line-by-Line

  * `#!/bin/bash`: Shebang to define interpreter
  * `echo`: Used to print output

## 3\. Working with Files and Directories

    
    
    mkdir new_project
    cd new_project
    ls
    touch file{1..5}.txt
    rm file1.txt
    rmdir new_project

![commands](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/3.png)

## 4\. Command-Line Power: Pipes and Redirection

    
    
    ls | grep "project"
    echo "Backup complete" > backup.log
    cat backup.log

![fourth](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/4.png)

## 5\. File Manipulation

![5](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/5.png)

    
    
    cat filename.txt
    head -n 2 filename.txt
    tail -n 3 filename.txt
    cp source.txt destination.txt
    mv oldname.txt newname.txt
    mv file.txt /path/to/directory/

![6](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/6.png)
![7](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/7.png)

## 6\. Working with Text Files

    
    
    grep "computing" file1.txt
    cut -c5-10 file1.txt
    paste file1.txt backup.log

![files](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/8.png)

## 7\. Variables and Substitution

    
    
    name="Waleed"
    echo "Hello, $name!"

## 8\. Input and Output

    
    
    read -p "Enter your city: " city
    echo "You live in $city"
    echo "This is a log entry" >> logfile.txt

![9](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/9.png)

## 9\. Control Structures

    
    
    read -p "Enter a number: " num
    if [ $num -gt 0 ]; then
        echo "Positive number"
    elif [ $num -lt 0 ]; then
        echo "Negative number"
    else
        echo "Zero"
    fi

![10](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/10.png)

## 10\. Bash Comparison Operators Cheat Sheet

Operator| Meaning  
---|---  
-gt| Greater Than  
-lt| Less Than  
-ge| Greater Than or Equal  
-le| Less Than or Equal  
-eq| Equal (numbers)  
-ne| Not Equal (numbers)  
==| Equal (strings)  
!=| Not Equal (strings)  
-z| Empty string  
-n| Non-empty string  
-w| Writable  
-x| Executable  
-r| Readable  
-d| Directory  
  
## 10\. Loops

### For Loop:

    
    
    for i in {1..5}; do
        echo "Number: $i"
    done

![11](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/11.png)

    
    
    for file in *.txt; do
        echo "Found file: $file"
        wc -l "$file"
    done
    
    
    for (( i=0; i<=5; i++ )); do
        power=$(( 2**i ))
        echo "2^$i = $power"
    done

![12](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/12.png)

### While Loop:

    
    
    count=1
    while [ $count -le 5 ]
    do
        echo "Count: $count"
        ((count++))
    done

![13](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/13.png)

## 11\. Functions

    
    
    greet() {
        echo "Welcome, $1!"
    }
    greet "Waleed"

![14](http://ee.uet.edu.pk/meds/wp-content/uploads/2025/06/14.png)

## 12\. Conclusion

This tutorial provides a complete beginner-friendly journey into shell
scripting, covering everything from basic commands to functions and loops.
Along the way, I included my own screenshots to make the concepts even easier
to understand and to support students in practical learning. With consistent
practice, you will be able to automate tasks and work confidently like a real
system engineer. Keep exploring and scripting, your Linux journey has just
begun!

## Further Resources

  * [Linux Handbook: Bash Test Operators](https://linuxhandbook.com/bash-test-operators/)
  * [Microsoft Shell Training](https://learn.microsoft.com/en-us/training/paths/shell/)
  * [MIT Missing Semester: Shell](https://missing.csail.mit.edu/2020/course-shell/)
  * [MIT Missing Semester: Shell Tools](https://missing.csail.mit.edu/2020/shell-tools/)

