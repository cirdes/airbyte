FROM python:3.10.12 as base


ENV ACCEPTANCE_TEST_DOCKER_CONTAINER 1
ENV DOCKER_VERSION = "24.0.2"

RUN apt-get update \
    && pip install --upgrade pip \
    && apt-get install tzdata bash curl

# Docker is required for the dagger in docker use case.
RUN curl -fsSL https://get.docker.com | sh



RUN pip install poetry==1.5.1
RUN poetry config virtualenvs.create false 
RUN echo "Etc/UTC" > /etc/timezone

WORKDIR /app
COPY pyproject.toml /app
COPY poetry.lock /app
RUN poetry install --no-root --only main --no-interaction --no-ansi
COPY . /app
RUN poetry install --only main --no-cache --no-interaction --no-ansi

LABEL io.airbyte.version=1.0.0
LABEL io.airbyte.name=airbyte/connector-acceptance-test
WORKDIR /test_input
ENTRYPOINT ["python", "-m", "pytest", "-p", "connector_acceptance_test.plugin", "-r", "fEsx", "--show-capture=log"]
