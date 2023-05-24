cmake_minimum_required(VERSION 3.2)
project(manipulating-cache-variables)

# Cache Variables are useful to configure our project either from cli or gui.
# I am guessing this means it integrates well with IDE's too, which is nice.

# Let's set up some cache variables
# Force means this is always set or updated. I have no idea how the INTERNAL one
# can avoid recomputations if it implies the use of FORCE. But it's probably
# explained later in the book.
set(pathToSomething "path/thing" CACHE PATH "This a path to something" FORCE)

# This won't be visible to the user. Sometimes used to cache the result of
# an internsive query or computation. GUI tools do not show INTERNAL variables.
# INTERNAL also implies FORCE
set(internalOne "internal!" CACHE STRING "This a path to something" INTERNAL)

# Initial value here is omitted, it defaults to FALSE
# Option ones are always cached I'm pretty sure, idk if we can pass FORCE
option(optVar1 BOOL "This is a checkbox in the GUI")
# More or less equivalent to this
set(optVar2 FALSE CACHE BOOL "This is a another checkbox in the GUI")
# Trying it with FORCE, does not seem to work. Is FORCE always implied here
# maybe. I'll look into this later on. OK it is not forced and cannot be set to
# FORCE.
# Here is the reason given in the book:
# The main reason for this is that cache variables are primarily intended as a
# customization point for developers. Rather than hard-coding the value in the
# CMakeLists.txt file as a normal variable, a cache variable can be used so
# that the developer can override the value without having to edit the
# CMakeLists.txt file. The variable can be modified by interactive GUI tools or
# by scripts without having to change anything in the project itself.
# option(optVar3 BOOL "This is a forced checkbox in the GUI" FORCE)


# Let's try opening this src directory with cmake-gui and see what options we
# have there. Nice I get the options in the GUI! And a mouse hove on the items
# displays the help strings which is sweet!
