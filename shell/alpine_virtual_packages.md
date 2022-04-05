# Alpine virtual packages

According to docs:

```
-t, --virtual NAME    Instead of adding all the packages to 'world', create a new 
                      virtual package with the listed dependencies and add that 
                      to 'world'; the actions of the command are easily reverted 
                      by deleting the virtual package
```

What that means is when you install packages, those packages are not added to global packages. And this change can be easily reverted. So if I need gcc to compile a program, but once the program is compiled I no more need gcc.

I can install gcc, and other required packages in a virtual package and all of its dependencies and everything can be removed this virtual package name. Below is an example usage:

```
RUN apk add --virtual mypacks gcc vim \
 && apk del mypacks
```

The next command will delete all 18 packages installed with the first command.
In docker these must be executed as a single RUN command (as shown above), otherwise it will not reduce the image size.
