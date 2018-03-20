# Build Stage
FROM rust AS build-stage

ADD . /usr/src/{{cookiecutter.project_name}}
WORKDIR /usr/src/myapp

RUN cargo build --release

# Final Stage
FROM scratch

ARG GIT_COMMIT
ARG VERSION
LABEL REPO="https://github.com/{{cookiecutter.github_username}}/{{cookiecutter.project_name}}"
LABEL GIT_COMMIT=$GIT_COMMIT
LABEL VERSION=$VERSION

WORKDIR /usr/local/bin

COPY --from=build-stage /usr/src/{{cookiecutter.project_name}}/bin/{{cookiecutter.project_name}} /opt/{{cookiecutter.project_name}}/bin/
RUN chmod +x /usr/local/bin/{{cookiecutter.project_name}}

CMD /usr/local/bin/{{cookiecutter.project_name}}
