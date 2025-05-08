FROM registry.access.redhat.com/ubi9/ubi-minimal:9.5-1745855087 AS build-venv

RUN microdnf update -y && \
    microdnf install -y python3.12 python3.12-pip

COPY requirements.txt requirements.txt

RUN python3.12 -m venv venv
RUN venv/bin/pip install --no-cache-dir -r requirements.txt

FROM gcr.io/distroless/python3-debian12:nonroot

COPY --from=build-venv /venv/ /venv/

WORKDIR /app

COPY __init__.py __init__.py
COPY app.py app.py
COPY event_manager.py event_manager.py
COPY main.py main.py
COPY path_finder.py path_finder.py
COPY resource_watcher.py resource_watcher.py
COPY taxonomy_creator.py taxonomy_creator.py
COPY topology_analyzer.py topology_analyzer.py
COPY topology_manager.py topology_manager.py

CMD ["/venv/bin/python", "main.py"]
