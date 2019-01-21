RELEASE := equestria
NAMESPACE := minecraft

.DEFAULT_GOAL := install

lint:
	find -type f -name '*.yaml' | xargs yamllint

template:
	helm template minecraft \
	--namespace=$(NAMESPACE) \
	-f values.yaml \


install: lint
	helm upgrade --install --force --wait $(RELEASE) \
	--namespace=$(NAMESPACE) \
	-f values.yaml \
	stable/minecraft

destroy: lint
	helm delete --purge $(RELEASE)
