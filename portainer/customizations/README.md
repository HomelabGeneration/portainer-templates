# Portainer customizations

Holding portainer customizations like a custom list of application templates.

## Portainer startup command

By using the following startup options you can use the customizations provided by the repository.

```
sudo docker run -d \
 -p 9443:9443 \
 --name portainer \
 -v portainer_data:/data \
 -v /var/run/docker.sock:/var/run/docker.sock \
 portainer/portainer-ee:latest \
 --templates https://github.com/HomelabGeneration/home-stacks/raw/main/portainer/customizations/templates.json \
 --logo https://github.com/HomelabGeneration/home-stacks/raw/main/portainer/customizations/portainer_logo.png \
 --http-disabled
```
