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
- top of makefile
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

# or

MAKEFILE_DIR := $(dir $(abspath $(lastword $(MAKEFILE_LIST))))
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

## Makefile help comments
* from nomad project - https://github.com/hashicorp/nomad/blob/main/GNUmakefile#L410
- top of makefile 2
```
# cyan (\033[36m) heading that's 32chars left-aligned (%-32s)
HELP_FORMAT="    \033[36m%-32s\033[0m %s\n"
.PHONY: help
help: ## Display this usage information
	@echo "Valid targets:"
	@grep -E '^[^ ]+:.*?## .*$$' $(MAKEFILE_LIST) | \
		sort | \
		awk 'BEGIN {FS = ":.*?## "}; \
			{printf $(HELP_FORMAT), $$1, $$2}'
	@echo ""
	@echo "This host will build the following targets if 'make release' is invoked:"
	@echo $(ALL_TARGETS) | sed 's/^/    /'
```
- w/var and dependent tgt - https://github.com/hashicorp/nomad/blob/v1.11.1/GNUmakefile#L294-L296
```
.PHONY: prerelease
prerelease: GO_TAGS=ui codegen_generated release
prerelease: generate-all ember-dist static-assets ## Generate all the static assets for a Nomad release
```

## Makefile template
- top of makefile 3
```
.DEFAULT_GOAL := help
.PHONY: help

help: ## Show this help
	@echo ""
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST) | sort
	@echo ""
```

## vscode

On **cspell** :
* don't show in 'Problems' pane
    - `"cSpell.diagnosticLevel": "Hint"`
    - https://stackoverflow.com/a/50322474
