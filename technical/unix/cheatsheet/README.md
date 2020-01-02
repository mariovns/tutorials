# Cheatsheet

Operation Type | Command | Description | Switches [enhancement] | Examples
--- | --- | --- | --- | -
Admin | who | tells about all the logged in user | am i [about the current user] | who
Admin | which | display the installed location of the software | | which java
Admin | uptime | last start time of system | | uptime
Admin | top | get the details of all the running process like task manager | | top
Admin | ps | get all the application processes running | -ef [process executed with command] | ps
File | touch | file creation | | touch filename
File | vi | open up the vi editor for the specified file | ESC [:q (quit), :w(save), :q!(quit without save), i(insert)] | vi filename
File | cat | file creation, display, content addition | | cat filename
File | rm | deletes the file from the location | -r [recursively] -f [removes folder] | rm filename
Directory | mkdir | make a directory in current folder with the specified name | -p [] | mkdir folder
Directory | ls | display all files in the folder | -l [details: permission,modified date], -a [hidden files], -t [sort by time], -r [reverse order] | ls -lrt
Directory | cd | change directory to specified folder or home if unspecified | ..[go to parent], | cd folder
Directory | pwd | displays 'p'resent 'w'orking 'd'irectory | | pwd
Terminal | clear | clears the screen or move the console cursor to top | | clear
Terminal | history | displays all the commands executed in this terminal | | history
Terminal | man | manual of commands | | man ps
