cmake_minimum_required(VERSION 3.2)
project(variable-basics)

# All variables in CMake are string insternally, lists are just a
# `;` separated string

# We can declare a variable like this
set(myVar a b c)
# This won't print that variable, it will print myVar as a string instead
message(myVar)
# This will print the variable as you'd expect, in this case it prints abc
# which is confusing as internally it's kind of a list and is stored as a;b;c
message(${myVar})

# We can overwrite variables
set(myVar c b a)
message(${myVar})

# This is the same as set(myVar a b c)
set(myVar a;b;c)
message(${myVar})

# Things get weird with this, these are all not the same thing but they print
# the same thing...
set(myVar abc)
message(${myVar})
set(myVar a b c)
message(${myVar})
set(myVar ab; c)
message(${myVar})

# This actually would print two new lines
# message("\n")
# Do this instead
message("")

set(foo ab)               # foo   = "ab"
set(bar   ${foo}cd)       # bar   = "abcd"
set(baz   ${foo} cd)      # baz   = "ab;cd"
set(myVar ba)             # myVar = "ba"
# Whooops and this makes big have some completely other value
# set(bar ${notSetVar})     # bar   = ""

# What in the what is it this?! Is it just a recursive thingy?
# Pure evil I must say.
set(big "${${myVar}r}ef")  # big   = "${bar}ef" = "abcdef"

# I'm guessing something like this is internally "foo = ;ab"
message("foo = " ${foo})
# Cannot stress enough how weird it feels that bar and baz aren't the same yet
# print the same thing. Maybe there's another function we learn later on that
# doesn't do this.
message("bar = " ${bar})
message("baz = " ${baz})
message("myvar = " ${myVar})
message("big = " ${big})
message("bar = " ${bar})

# More cursed stuff. This prints the second line on a newline, but there is no
# explicit newline character. I wonder what this evaluates to internally...
# Is it just "First line goes here\nSecond line with a \"quoted\" word"?
set(muVar "goes here")
set(multiLine "First line ${myVar}
Second line with a \"quoted\" word")
message(multiLine = ${multiLine})

# In CMake 3.0 or later we have these:

# This looks like it behaves what you look is what you get when newline and
# and spaces in front matter. No substitutions are made.
set(multiLine [[
First line
  Second line
  "Quoted \nthingy"
  ${baz}
]])
message(multiLine = ${multiLine})

# These seem identical so why the different syntax?
set(multiLine [=[
First line
  Second line
  "Quoted \nthingy"
  ${baz}
]=])
message(multiLine = ${multiLine})

# OK now I see why this is a thing, in this use case it's actually really nice.
set(shellScript [=[
  #!/bin/bash

  [[ -n "${USER}" ]] && echo "Have USER"
]=])
message(shellScript = ${shellScript})

# We can unset a variable by doing wither of these, these are equivalent and do
# not throw errors or warnings if the variable does not already exists.
set(myVar)
unset(myVar)

# Environment Variables

# We can set an ENV varaible like this
# This is rarely useful as it only exist in that CMAke instance. Won't even be
# visible in build time.
set(ENV{PATH} "$ENV{PATH}:/opt/myDir")

# Cache Variables are onlu set one until something explicitly removes them from
# the cache.
# set(varName value... CACHE type "docstring" [FORCE])
# Let's leave this one for later as it gets talked about more in depth
# What would happen if we try to overwrite a non cache varaible?

# Scope Blocks
# Cache varaible have global scope. All the other examples above have local file
# scope, meaning the currect CMakeLists.txt file. This is often called the
# directory scope. Subdirectories and functions inherit from their parnt scope.

# For scope and policy stuff read sections 5.4 amd 5.5.
