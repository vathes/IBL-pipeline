# Containerized IBL Environment

This is a containerized IBL environment for ingestion of IBL data from Alyx/flatiron to  DataJoint

## Docker build

The Dockerfile makes use of `buildx`. You may need to set this up before trying to build the image.

```bash
docker buildx install
docker buildx create --use
```

Then from the `IBL-pipeline` directory: 

```bash
cd docker/iblenv
docker buildx build \
    --file=Dockerfile \
    --push \
    --platform=linux/amd64,linux/arm64 \
    --target=iblenv_alyx \
    --tag=iamamutt/iblenv_alyx:v1.0.0 \
    --tag=iamamutt/iblenv_alyx:latest \
    --build-arg GROUPNAME=ibl \
    --build-arg USERNAME=ibluser \
    --build-arg USER_GID=1000 \
    --build-arg USER_UID=1000 \
    --build-arg CONDA_ENV_FILE=iblenv.dj.yml \
    .
```

<!--
local docker image (single platform only)

```bash
docker build \
    --file=Dockerfile \
    --output=type=docker \
    --platform=linux/arm64 \
    --target=iblenv_alyx \
    --tag=iblenv_alyx:v1.0.0 \
    --tag=iblenv_alyx:latest \
    --build-arg GROUPNAME=ibl \
    --build-arg USERNAME=ibluser \
    --build-arg USER_GID=1000 \
    --build-arg USER_UID=1000 \
    --build-arg CONDA_ENV_FILE=iblenv.dj.yml \
    .
```

Push image to Docker Hub

```bash
docker tag iblenv_alyx:v1.0.0 iamamutt/iblenv_alyx:v1.0.0
docker tag iblenv_alyx:latest iamamutt/iblenv_alyx:latest
docker push iamamutt/iblenv_alyx:latest
```
-->

The environment variable `CONDA_ENV_FILE` allows for using a different conda environment file. Leave blank to use the default iblenv.yaml from the iblenv GitHub repo. If the standard iblenv.yaml setup fails, try a different one such as: `docker/iblenv/iblenv.dj.yml`. The path should be relative to the build context.

Change `--platform` to a valid architecture name to use another platform when building, e.g., `linux/aarch64`.