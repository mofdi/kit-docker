#### Begin of system configuration section. ####

export DOCKER_COMPOSE_ROOT ?= "$(shell pwd)"
export CI_PROJECT_NAME = gcobehat
export PWD = pwd

# Docker compose configurations
COMPOSE_FILE ?= $(DOCKER_COMPOSE_ROOT)/docker-compose.yml
COMPOSE_BIN ?= docker-compose -f $(COMPOSE_FILE) -p ${CI_PROJECT_NAME}

# Services
COMPOSE_SERVICES_FILE ?= $(DOCKER_COMPOSE_ROOT)/docker-compose.services.yml
COMPOSE_SERVICES_BIN ?= docker-compose -f $(COMPOSE_SERVICES_FILE) -p $(CI_PROJECT_NAME)
NPM_SERVICES_BIN ?= docker-compose -f $(COMPOSE_SERVICES_FILE) -p $(CI_PROJECT_NAME)

COMPOSER_BIN ?= $(COMPOSE_SERVICES_BIN) run --rm composer
NPM_BIN ?= $(NPM_SERVICES_BIN) run --rm npm

#### End of system configuration section. ####

up:
	@$(COMPOSE_BIN) up -d

stop:
	@$(COMPOSE_BIN) stop

ip:
	@echo "GATEWAY : " $(shell docker network inspect -f '{{range .IPAM.Config}}{{.Gateway}}{{end}}' ${CI_PROJECT_NAME}_default)
	@for CONTAINER in $(shell docker ps -a --format '{{.Names}}' -f NAME=${CI_PROJECT_NAME}); do \
	  echo "$$CONTAINER#$$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $$CONTAINER)"; \
	done | column -t -s "#"

composer:
	@$(COMPOSER_BIN) ${CMD}

npm:
	@$(NPM_BIN) ${CMD}

behat:
	@make composer CMD="run-script run-behat -- ${CMD}"
