SHELL := /bin/bash

.PHONY: help
.DEFAULT_GOAL := help

help: ## 💬 This help message :)
	@grep -E '[a-zA-Z_-]+:.*?## .*$$' $(firstword $(MAKEFILE_LIST)) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

build: ## 🔨 Build the Banking Application
	@echo -e "\e[34m$@\e[0m" || true
	@npm run build

test: build ## 🧪 Test the Banking Application in the sandbox
	@echo -e "\e[34m$@\e[0m" || true
	@. ./scripts/test_sandbox.sh --nodeAddress 127.0.0.1:8000 --certificate_dir ./workspace/sandbox_common

test-docker-virtual: build-virtual ## 🧪 Test the Banking Application in a Docker sandbox
	@echo -e "\e[34m$@\e[0m" || true
	@. ./scripts/test_docker.sh --virtual --serverIP 172.17.0.3 --port 8080

test-docker-enclave: build-enclave ## 🧪 Test the Banking Application in a Docker enclave
	@echo -e "\e[34m$@\e[0m" || true
	@. ./scripts/test_docker.sh --enclave --serverIP 172.17.0.4 --port 8080

# Run sandbox. Consider 3 members as 3 banks.
# This is used in the demo scripts
start-host: build ## 🏁 Start the CCF Sandbox for the demo
	@echo -e "\e[34m$@\e[0m" || true
	@/opt/ccf/bin/sandbox.sh --js-app-bundle ./dist/ --initial-member-count 3 --initial-user-count 0 --constitution-dir ./constitution

demo: ## 🎬 Demo the Banking Application
	@echo -e "\e[34m$@\e[0m" || true
	@./scripts/demo.sh
	@./scripts/show_app_log.sh

clean: ## 🧹 Clean the working folders created during build/demo
	@rm -rf .venv_ccf_sandbox
	@rm -rf .venv_ccf_verify_receipt
	@rm -rf workspace
	@rm -rf dist

build-virtual: build ## 📦 Build Virtual container image from Dockerfile
	@echo -e "\e[34m$@\e[0m" || true
	@../build_image.sh virtual

build-enclave: build ## 📦 Build Enclave container image from Dockerfile
	@echo -e "\e[34m$@\e[0m" || true
	@../build_image.sh enclave
