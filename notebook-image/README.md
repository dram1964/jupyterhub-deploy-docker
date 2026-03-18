# Notebook Image

JupyterLab Notebook used by the JupyterHub Spawner. 

## Build Instructions

Build this image with: 

```
docker compose build
```

## Test Image

You can run a bash shell in this image with: 

```
docker container run -it --rm --user root --entrypoint /bin/bash ${image_name}:${image_tag}
```