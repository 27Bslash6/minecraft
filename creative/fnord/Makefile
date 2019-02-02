mkfile_path := $(abspath $(lastword $(MAKEFILE_LIST)))
current_dir := $(notdir $(patsubst %/,%,$(dir $(mkfile_path))))

# Takes name of release from current folder
RELEASE := $(current_dir)

# =============================================================================

GCP_PROJECT := cedar-hawk-222400

GKE_CLUSTER_NAME := hosting-base
GKE_CLUSTER_ZONE := us-central1-a

NAMESPACE := minecraft
CONTEXT := minecraft

.DEFAULT_GOAL := all

all: install ip

auth:
	gcloud container clusters get-credentials $(GKE_CLUSTER_NAME) --zone $(GKE_CLUSTER_ZONE) --project $(GCP_PROJECT)

context: auth
	kubectl config use-context minecraft

lint:
	find -type f -name '*.yaml' | xargs yamllint -d "{extends: relaxed, rules: {line-length: {max: 120}}}"

template: lint
	helm template minecraft \
	--namespace=$(NAMESPACE) \
	-f values.yaml

install: context lint
	helm upgrade --install --force --wait $(RELEASE) \
	--namespace=$(NAMESPACE) \
	-f values.yaml \
	stable/minecraft

status:
	helm status $(RELEASE)

delete: context lint
	helm delete $(RELEASE)

purge: context lint
	helm delete --purge $(RELEASE)

ip:
	@printf "%s\n" $(shell kubectl get svc --namespace $(NAMESPACE) $(RELEASE)-$(NAMESPACE) -o jsonpath="{.status.loadBalancer.ingress[*].ip}")
