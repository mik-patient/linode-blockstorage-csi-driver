export GO111MODULE=on

IMAGE_VERSION=canary
K8S_VERSION?=""

.PHONY: test clean check-token install-terraform test-manifest

test-manifest:
	../hack/generate-yaml.sh $(IMAGE_VERSION) > test/manifest/linode-blockstorage-csi-driver.yaml

test: $(GOPATH)/bin/ginkgo check-token test-manifest
	go list -m; \
	ginkgo -r --v --progress --trace --cover $(TEST_ARGS) -- --v=3 --k8s-version=${K8S_VERSION} $(SUITE_ARGS)

clean: check-token
	cd test; \
	./scripts/delete_cluster.sh csi-linode-for-reuse

check-token:
	@if test "$(LINODE_API_TOKEN)" = "" ; then \
	  echo "LINODE_API_TOKEN must be set"; \
	  exit 1; \
	fi

install-terraform:
	sudo apt-get install wget unzip
	wget https://releases.hashicorp.com/terraform/0.11.13/terraform_0.11.13_linux_amd64.zip
	unzip terraform_0.11.13_linux_amd64.zip
	sudo mv terraform /usr/local/bin/
