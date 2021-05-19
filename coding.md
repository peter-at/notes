# coding.md

# gnu makefile

## multiple target matches
first match is taken - [look at what.ever target](https://clarkgrubb.com/makefile-style-guide#rule-target-decl)
style recommendation - use `%.extension`

## useful settings (top of makefile)
top of makefile
```
MAKEFLAGS += --warn-undefined-variables
SHELL := bash
.SHELLFLAGS := -eu -o pipefail -c
.DEFAULT_GOAL := all
.DELETE_ON_ERROR:
.SUFFIXES:
```