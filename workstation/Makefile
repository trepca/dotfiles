GITCOMMIT := $(shell git rev-parse --short=7 HEAD 2>/dev/null)
NAME=dev

all: build

build:
	docker build -t trepca/${NAME}:${GITCOMMIT} .

push:
	@echo "==> Publishing trepca/${NAME}:$(GITCOMMIT)"
	@docker tag trepca/${NAME}:$(GITCOMMIT) trepca/${NAME}:latest
	@docker push trepca/${NAME}:$(GITCOMMIT)
	@docker push trepca/${NAME}:latest
	@echo "==> Your image is now available at trepca/${NAME}:$(GITCOMMIT)"

run: kill
	docker run -it -h dev -d --net=host --rm -v /var/run/docker.sock:/var/run/docker.sock -v /home/trepca/code:/home/trepca/code -v /home/trepca/.ssh:/home/trepca/.ssh -v /home/trepca/.zsh_private:/home/trepca/.zsh_private -v /home/trepca/.zsh_history:/home/trepca/.zsh_history --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --privileged --name dev trepca:dev 

ssh:
	mosh --no-init --ssh="ssh -o StrictHostKeyChecking=no -i ~/.ssh/github_rsa -p 3222" trepca@localhost -- tmux new-session -AD -s main

kill:
	docker kill dev | true


.PHONY: all build run kill
