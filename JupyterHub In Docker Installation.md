# JupyterHub In Docker Installation

## References

- https://docs.docker.com/guides/jupyter/#customize-your-jupyterlab-environment
- https://jupyter.org/hub
- https://jupyterhub.readthedocs.io/en/latest/explanation/concepts.html
- https://jupyterhub.readthedocs.io/en/latest/reference/authenticators.html
- https://jupyterhub.readthedocs.io/en/latest/reference/technical-overview.html
- https://jupyterlab.readthedocs.io/en/latest/getting_started/overview.html
- https://discourse.jupyter.org/t/jupyterhub-with-docker-spawner/31440
- https://github.com/dram1964/jupyterhub-deploy-docker.git


## Notes

1. Use `quay.io/jupyter/datascience-notebook:latest`
    - see https://quay.io/repository/jupyter/datascience-notebook?tab=tags&tag=latest
    - see https://github.com/jupyter/docker-stacks/tree/main/images
    
## Configuration Changes

Changes to the `jupyterhub_config.py` will only appear by first stopping and removing the container 
and then restarting it: 

```
docker compose stop 
docker compose rm
docker compose up -d
```

## Admin

Usernames for admins are defined in the `jupyterhub_config.py`. However, the accounts are
not created. Instead each admin user needs to sign-up. 

Once signed up, an admin can connect to the following URLs: 

1. `/hub/home`
2. `/hub/token`
3. `/hub/admin`
4. `/hub/authorize`

## Users

Users can go to `/hub/change-password` to change their password. 

## Notebooks 

Notebooks are accessible on `http://localhost:8000/user/<username>/lab`. 

## Authentication with Native Authenticator

[Full Documentation](https://native-authenticator.readthedocs.io/en/latest/options.html)
for Native Authenticator.
