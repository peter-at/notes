# coding.md

## == gnu makefile ==

## Multiple target matches
first match is taken - [look at what.ever target](https://clarkgrubb.com/makefile-style-guide#rule-target-decl)
```
# content of Makefile
cat << EOF > Makefile
what.%:
	echo what

what.%:
	echo what v2

%.ever:
	echo ever
EOF

# call make - first pattern that matches will be executed...  if the pattern is exactly the same, it overwrites the previous rule
% make what.ever
echo what v2
what v2
```

style recommendation - use `%.extension`

## Check if varialbe is set
```
.chk-var:
ifndef VAR
  $(error VAR is not set)
endif
```

## Useful settings (top of makefile)
top of makefile
```
MAKEFLAGS += --warn-undefined-variables
SHELL := bash
.SHELLFLAGS := -eu -o pipefail -c
.DEFAULT_GOAL := all
.DELETE_ON_ERROR:
.SUFFIXES:

all : help ;

PHONY_TGTS := help
PHONY_TGTS += network
.PHONY: $(PHONY_TGTS)

help:
	@echo "**** Help ****"
	@grep -E "(^# make|^#\s+-)" Makefile

# make example
#    - additional help notes for example target
example:
  @:
```

## Print variables
```
SHOW_VARS_TGTS:=$(addprefix print-,VAR1 VAR2 VAR3)

show-vars : $(SHOW_VARS_TGTS) ;

print-% : ; $(info info: $* is a $(flavor $*) variable set to [$($*)]) @true
```

## Full path of Makefile's directory
```
BASEDIR:=$(dir $(realpath $(firstword $(MAKEFILE_LIST))))
BASEDIR:=$(patsubst %/,%,$(BASEDIR))
```

## Share makefiles
```
SOMEVAR := foo
# common vars
-include ../common-vars.mk

# targets base on common targets
all: common.tgt1

release: common.do-release

local-target:
  echo "local target"

-include ../common-tgts.mk
```

## Targets

.. as if '@' is used for all the commands
`$(VERBOSE).SILENT:`

.. get docker image date
```
## helper template function
_img_date = $(shell docker image inspect $(1) \                                                  
  | jq '.[0].Created | sub(".[0-9]+Z"; "Z")' \
  | jq 'fromdate|strftime("%Y.%m.%d")')  

_LOC_LATEST = $(eval _LOC_LATEST := $$(call _img_date,$(IMAGE):latest))$(_LOC_LATEST)
_REG_LATEST = $(eval _REG_LATEST := $$(call _img_date,$(REGISTRY)/$(IMAGE):latest))$(_REG_LATEST)
```
