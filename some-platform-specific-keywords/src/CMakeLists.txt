cmake_minimum_required(VERSION 3.2)
project(platform_keywords)
# These options get ignored if not on those platforms
add_executable(platform_keywords WIN32 MACOSX_BUNDLE
  main.c
)

