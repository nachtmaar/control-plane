OAPI_GENERATOR=oapi-codegen
OAPI_FILE=./docs/external_api.yaml
OAPI_REVISION='be3fe478e9fdca2baea8cedfd661f5a9119a0052'

CLIENT_FILE=./pkg/client_gen.go
TYPES_FILE=./pkg/types_gen.go

docs/external_api.yaml:
	curl -o $(OAPI_FILE) --create-dirs https://raw.githubusercontent.com/kyma-incubator/reconciler/$(OAPI_REVISION)/openapi/external_api.yaml

.PHONY: generate-oapi-artifacts
generate-oapi-artifacts: docs/external_api.yaml
	$(OAPI_GENERATOR) -generate 'types' -o $(TYPES_FILE) -package mothership ./docs/external_api.yaml
	$(OAPI_GENERATOR) -generate 'client' -o $(CLIENT_FILE) -package mothership ./docs/external_api.yaml

.PHONY: generate-mothership-client
generate-mothership-client:
	mockgen -destination=pkg/automock/client.go -package=automock -source=pkg/client_gen.go Client

.PHONY: clean
clean:
	rm $(OAPI_FILE) $(CLIENT_FILE) $(TYPES_FILE) || true

.PHONY: all
all: clean generate-oapi-artifacts generate-mothership-client

.DEFAULT_GOAL=all
