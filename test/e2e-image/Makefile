
DIR:=$(shell dirname $(realpath $(firstword $(MAKEFILE_LIST))))
E2E_BASE_IMAGE ?= "registry.k8s.io/ingress-nginx/e2e-test-runner:v2.0.2@sha256:192e9e4b4f660093a3bc73e1a5e702ee287eb2a351f38cdc5f72e1ff94e65e31"

image:
	echo "..entered Makefile in /test/e2e-image"
	echo "..calling Make target <<e2e-test-binary>> in /Makefile from inside /test/e2e-image/Makefile"
	make -C $(DIR)/../../ e2e-test-binary
	echo "..done building e2e-test-binary from /test/e2e-image/Makefile"

	cp $(DIR)/../e2e/e2e.test .
	cp $(DIR)/../e2e/wait-for-nginx.sh .
	cp -R $(DIR)/../../charts .

	# TODO: avoid manual copy
	cp -R $(DIR)/../../test/e2e/settings/ocsp/* .

	docker build \
		--build-arg E2E_BASE_IMAGE=$(E2E_BASE_IMAGE) \
		--tag nginx-ingress-controller:e2e .

clean:
	rm -rf _cache e2e.test kubectl cluster ginkgo
	docker rmi -f nginx-ingress-controller:e2e || true


.PHONY: image clean
