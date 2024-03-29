# Portainer Templates

A compiled list ready to go Portainer App templates

## Usage

1. Log into your Portainer web UI
2. Under <ins>Settings</ins> --> <ins>App Templates</ins>, update the URL to
   `https://raw.githubusercontent.com/HomelabGeneration/home-stacks/main/templates/templates.json`
3. Now under <ins>Home</ins> --> <ins>App Templates</ins>, you should see all apps. Click one to deploy.

<details>
<summary>Show me...</summary>

<p align="center"><img width="800" src="https://i.ibb.co/XxGRjrs/portainer-templates-installation.gif" /></p>

</details>

Alternatively, when you start Portainer, you can append the `--templates` flag pointing to the templates URL.
```
sudo docker run -d \
 -p 9443:9443 \
 --name portainer \
 --restart unless-stopped \
 -v portainer_data:/data \
 -v /var/run/docker.sock:/var/run/docker.sock \
 portainer/portainer-ee:latest \
 --templates https://raw.githubusercontent.com/HomelabGeneration/home-stacks/main/templates/templates.json \
 --logo https://raw.githubusercontent.com/HomelabGeneration/home-stacks/main/templates/portainer_logo.png \
 --http-disabled
```
