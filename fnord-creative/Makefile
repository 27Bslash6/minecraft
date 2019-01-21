RELEASE := fnord
NAMESPACE := minecraft

.DEFAULT_GOAL := install

lint:
	find -type f -name '*.yaml' | xargs yamllint

template: lint
	helm template minecraft \
	--namespace=$(NAMESPACE) \
	-f values.yaml \

install: lint
	helm upgrade --install --force --wait $(RELEASE) \
	--namespace=$(NAMESPACE) \
	-f values.yaml \
	stable/minecraft

delete: lint
	helm delete $(RELEASE)

purge: lint
	helm delete --purge $(RELEASE)
