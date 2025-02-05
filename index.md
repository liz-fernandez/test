---
layout: page
title: BABS Honours Bioinformatics Course 2024
---

The Unix shell has been around longer than most of its users have been alive.
It has survived so long because it's a power tool
that allows people to do complex things with just a few keystrokes.
More importantly,
it helps them combine existing programs in new ways
and automate repetitive tasks
so that they don't have to type the same things over and over again.
Use of the shell is fundamental to using a wide range of other powerful tools
and computing resources (including "high-performance computing" supercomputers).
These lessons will start you on a path towards using these resources effectively.


> ## Prerequisites {.prereq}
>
> This lesson guides you through the basics of file systems and the
> shell.  If you have stored files on a computer at all and recognize
> the word “file” and either “directory” or “folder” (two common words
> for the same thing), you're ready for this lesson.
>
> If you're already comfortable manipulating files and directories,
> searching for files with `grep` and `find`, and writing simple loops
> and scripts, you probably won't learn much from this lesson.

> ## Getting ready {.getready}
> 
> We will be using the Katana server. A very simple way to access Katana is to 
> use the 'Katana OnDemand' version. 
>
> 1. Navigate to the following link:
> [Katana On Demand](https://kod.restech.unsw.edu.au/)
> 2. Click on the 'FastX Desktop' button. 
> 3. Click on the 'Launch' button. 
> 4. Click on the 'Connect to FastX Desktop' button. 
> 5. Double click on the folder named 'yourzID's Home'
> 5. Make a new folder in your Desktop called 'Bioinfo_Course' and double click it.
> 6. Download data-shell-dir.zip by opening and internet browser and navigating to the following url:
> [https://liz-fernandez.github.io/Honours_Bioinfo_2024/DATA/data-shell-dir.zip](https://liz-fernandez.github.io/Honours_Bioinfo_2024/DATA/data-shell-dir.zip)
> 7. Move the 'data-shell-dir.zip' file inside the 'Bioinfo_Course' folder.
> 8. Close your FastX Desktop browser tab. 
> 9. Click the red 'Delete' button. 
> 10. Navigate back to the [Katana OnDemand Dashboard](https://kod.restech.unsw.edu.au/pun/sys/dashboard) and click
>     on the 'Katana Terminal' button. 
> 11. Navigate to 'Bioinfo_Course' and decompress the file by copying the following lines and pasting them into the
>     terminal:
>
> ~~~ {.bash}
> cd Bionfo_Course
> unzip data-shell-dir.zip
> cd data-shell
> ~~~
> 
> You are now ready to start! 
> 
> You can get more support for Katana OnDemand [here](https://docs.restech.unsw.edu.au/using_katana/ondemand/)
>

## Topics

### Week 1 - Linux Shell

1.  [Introducing the Shell](00-intro_Shell.html)
2.  [Files and Directories](01-filedir_Shell.html)
3.  [Creating Things](02-create_Shell.html)
4.  [Pipes and Filters](03-pipefilter_Shell.html)
5.  [Loops](04-loop_Shell.html)
6.  [Shell Scripts](05-script_Shell.html)
7.  [Finding Things](06-find_Shell.html)

### Week 2 - Introduction to Python

[Python setup](00-setup_Python.html)

1.  [Python fundamentals](01-intro_Python.html)
2.  [Analyzing Patient Data](02-numpy_Python.html)
3.  [Visualising Tabular Data](03-matplotlib_Python.html)
4.  [Storing Multiple Values in Lists](04-lists_Python.html)
5.  [Repeating Actions with Loops](05-loop_Python.html)
6.  [Analyzing Data from Multiple Files](06-files_Python.html)
7.  [Making Choices](07-cond_Python.html)
8.  [Creating Functions](08-func_Python.html)
9.  [Errors and Exceptions](09-errors_Python.html)
10. [Defensive Programming](10-defensive_Python.html)
11. [Debugging](11-debugging_Python.html)
12. [Command-Line Programs](12-chtmlline_Python.html)

### Week 3 - Introduction to R

1.  [Introduction to R and RStudio](01-rstudio-intro_R.html)
2.  [Project Management With RStudio](02-project-intro_R.html)
3.  [Seeking Help](03-seeking-help_R.html)
4.  [Data Structures](04-data-structures-part1_R.html)
5.  [Exploring Data Frames](05-data-structures-part2_R.html)
6.  [Subsetting Data](06-data-subsetting_R.html)
7.  [Control Flow](07-control-flow_R.html)
8.  [Creating Publication-Quality Graphics with ggplot2](08-plot-ggplot2_R.html)
9.  [Vectorization](09-vectorization_R.html)
10. [Functions Explained](10-functions_R.html)
11. [Writing Data](11-writing-data_R.html)

