# Must be first since also makes sure we support the correct version
# of cmake but that we a have a working compiler and a linker
cmake_minimum_required(VERSION 3.2)
project(minimal_example)
add_executable(minimal_example main.c)
