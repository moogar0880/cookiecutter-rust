{% if cookiecutter.project_type == "service" -%}
BIN_NAME={{cookiecutter.project_name}}

VERSION=$(shell git describe --tags 2> /dev/null || echo '0.0.0')
GIT_COMMIT=$(shell git rev-parse HEAD)
GIT_DIRTY=$(shell test -n "`git status --porcelain`" && echo "+CHANGES" || true)
PRERELEASE=

# if we have untagged commits, mark this build as a pre-release
ifneq ($(strip $(GIT_DIRTY)),)
PRERELEASE=DEV
endif
{% endif -%}

{%- if cookiecutter.project_type == "service" -%}
.PHONY: all
all: clean vendor binary style test
{% else -%}
.PHONY: all
all: clean vendor style test
{% endif %}

.PHONY: help
help:
	@echo 'Management commands for {{cookiecutter.project_name}}:'
	@echo
	@echo 'Usage:'
	@echo '    make clean           Clean the directory tree.'
	@echo '    make test            Run tests on the project.'
	@echo '    make test/benchmark  Run benchmark tests on the project.'
	@echo '    make vendor          ensures dependencies are installed.'
	{% if cookiecutter.project_type == "service" -%}
	@echo '    make binary          Compile the binary for this project.'
	@echo '    make package         Build final docker image with just the go binary inside'
	@echo '    make push            Push tagged images to registry'
	@echo '    make tag             Tag image created by package with latest, git commit and version'
	{% endif -%}
	@echo

##############################################################################
# The following targets are used for aiding in development and CI for the 
# {{cookiecutter.project_name}} source code
##############################################################################
.PHONY: clean
clean:
	cargo clean

.PHONY: style
style:
	rustfmt --version
    cargo fmt -- --write-mode=diff

.PHONY: test
test:
	cargo test

.PHONY: test/benchmark
test/benchmark:
	cargo bench

.PHONY: vendor
vendor:
	cargo update

{% if cookiecutter.project_type == "service" -%}
##############################################################################
# The following targets are used for packaging the {{cookiecutter.project_name}}
# binary into a docker container
##############################################################################
.PHONY: binary
binary:
	@echo "building ${BIN_NAME} ${VERSION}"
	@echo "GOPATH=${GOPATH}"
	cargo build

.PHONY: package
package:
	@echo "building image ${BIN_NAME} ${VERSION} $(GIT_COMMIT)"
	docker build --build-arg VERSION=${VERSION} --build-arg GIT_COMMIT=$(GIT_COMMIT) -t $(IMAGE_NAME):local .

.PHONY: tag
tag: 
	@echo "Tagging: latest ${VERSION} $(GIT_COMMIT)"
	docker tag $(IMAGE_NAME):local $(IMAGE_NAME):$(GIT_COMMIT)
	docker tag $(IMAGE_NAME):local $(IMAGE_NAME):${VERSION}
	docker tag $(IMAGE_NAME):local $(IMAGE_NAME):latest

.PHONY: push
push: tag
	@echo "Pushing docker image to registry: latest ${VERSION} $(GIT_COMMIT)"
	docker push $(IMAGE_NAME):$(GIT_COMMIT)
	docker push $(IMAGE_NAME):${VERSION}
	docker push $(IMAGE_NAME):latest
{%- endif -%}
