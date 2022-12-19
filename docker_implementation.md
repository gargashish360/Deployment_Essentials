# How to structure a docker file:

* Seperate all the parameters, variables/ constants under seperate conf folder(Make yaml files).
* Place all the relevant data under data folder.
* Place docker files under docker folder:__
          --> Structuring a basic docker folder:
				Dockerfile:
				ARG baseImage
				ARG baseImageTag
				FROM ${baseImage}:${baseImageTag}
				WORKDIR /app
				COPY docker/requirements.txt /tmp/
				RUN pip3 install -r /tmp/requirements.txt && \
					rm /tmp/requirements.txt
				COPY src /app/src                               # This contains the main code files.
				COPY data /app/data
				COPY conf /app/conf
				COPY arm-go /app/conf
				CMD ["python", "/app/src/main.py"]
				
				prometheus& grafana: To collect the system metrics
				
				docker-compose.yaml: If you want to run several services and check their interactions.
				
				requirements.in: Contains all the requirement libraries.
* Put all the code need to be implemented under the src folder.
* Write the Makefile.
	Makefile:
	export BASE_IMAGE := neo-docker-releases.repo.lab.pl.alcatel-lucent.com/ubi-python-minimal
	export BASE_IMAGE_TAG := 3.9-ubi9
	.PHONY: compile-deps
    compile-deps:
			pip-compile docker/requirements.in
	.PHONY: build
	build:
			docker build --pull -f docker/Dockerfile -t cm-parameter-grouping --build-arg baseImage=$(BASE_IMAGE) --build-arg baseImageTag=$(BASE_IMAGE_TAG) .
	.PHONY: service-start
	service-start:
			docker run --name cm_parameter_grouping_contrainer -d cm-parameter-grouping
	.PHONY: logs
	logs:
			docker logs -f cm_parameter_grouping_contrainer
