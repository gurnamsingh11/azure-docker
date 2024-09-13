<h1>Docker and Azure Command Documentation</h1>

<h2>Build Docker Image</h2>

<h4>Build a Docker image from a Dockerfile located in the current directory. Tag the image for later use.</h4>

```
docker build --tag <image_name>:<tag> .
```

<h4>Example:</h4>

```
docker build --tag my-fastapi-app:test12 .
```

<h3>Run Docker Container</h3>

<h4>Run a Docker container in detached mode. Map port 80 on the host to port 80 in the container.

</h4>

```
docker run -d --name <container_name> -p 80:80 <image_name>:<tag>
```

<h4>Example</h4>

```
docker run -d --name my-fastapi-container -p 80:80 my-fastapi-app:test12
```

<h2>Azure CLI Commands</h2>

<h3>Login to Azure</h3>

<h4>Authenticate and log in to your Azure account.

</h4>

```
az login
```

<h3>Create a Resource Group</h3>

<h4>Create a Resource Group</h4>

```
az group create --location <location> --name <resource_group_name>
```

<h4>Example</h4>

```
az group create --location eastus --name my-resource-group
```

<h3>Delete a Resource Group</h3>

<h4>Delete a Resource Group</h4>

```
az group delete --name <resource_group_name>
```

<h4>Example</h4>

```
az group delete --name my-resource-group
```

<h3>Create a Container Registry</h3>

<h4>Create a new container registry within a specified resource group.

</h4>

```
az acr create --resource-group <resource_group_name> --name <registry_name> --sku <sku>
```

<h4>Example</h4>

```
az acr create --resource-group my-resource-group --name mycontainerregistry --sku Basic
```

<h3>Enable Admin Access for Container Registry</h3>

```
az acr update -n <registry_name> --admin-enabled true
```

<h4>Example</h4>

```
az acr update -n mycontainerregistry --admin-enabled true
```

<h3>Show Container Registry Credentials</h3>

```
az acr credential show --name <registry_name>
```

<h4>Example</h4>

```
az acr credential show --name mycontainerregistry
```


<h3>Log in to Container Registry</h3>

```
docker login <registry_url>
```

<h4>Example</h4>

```
docker login mycontainerregistry.azurecr.io
```

<h3>Build and Push Docker Image to Container Registry</h3>

```
az acr build --platform <platform> -t <registry_url>/<image_name>:<tag> -r <registry_name> .
```

<h4>Example</h4>

```
az acr build --platform linux/amd64 -t mycontainerregistry.azurecr.io/my-fastapi-app:latest -r mycontainerregistry .
```

<h3>Create a Container App Environment</h3>

```
az containerapp env create --name <environment_name> --resource-group <resource_group_name> --location <location>
```

<h4>Example</h4>

```
az containerapp env create --name my-fastapi-env --resource-group my-resource-group --location eastus
```

<h3>Create a Container App</h3>

```
az containerapp create --name <app_name> \
    --resource-group <resource_group_name> \
    --image <registry_url>/<image_name>:<tag> \
    --environment <environment_name> \
    --registry-server <registry_url> \
    --registry-username <registry_username> \
    --registry-password <registry_password> \
    --ingress external \
    --target-port 80
```

<h4>Example</h4>

```
az containerapp create --name my-fastapi-app \
    --resource-group my-resource-group \
    --image mycontainerregistry.azurecr.io/my-fastapi-app:latest \
    --environment my-fastapi-env \
    --registry-server mycontainerregistry.azurecr.io \
    --registry-username mycontainerregistry \
    --registry-password <password_here> \
    --ingress external \
    --target-port 80
```

<h3>Update and Redeploy</h3>

```
docker build --tag <image_name>:<tag> .
```

<h4>Example</h4>

```
docker build --tag my-fastapi-app:test12 .
```

<h3>Push Updated Image to Container Registry</h3>

```
az acr build --platform <platform> -t <registry_url>/<image_name>:<tag> -r <registry_name> .
```

<h4>Example</h4>

```
az acr build --platform linux/amd64 -t mycontainerregistry.azurecr.io/my-fastapi-app:latest -r mycontainerregistry .
```

<h3>Redeploy Updated Image</h3>

```
az containerapp revision copy --name <app_name> --resource-group <resource_group_name> --image <registry_url>/<image_name>:<tag>
```

<h4>Example</h4>

```
az containerapp revision copy --name my-fastapi-app --resource-group my-resource-group --image mycontainerregistry.azurecr.io/my-fastapi-app:latest
```











