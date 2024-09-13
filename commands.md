<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker and Azure Command Documentation</title>
</head>
<body>

<h1>Docker and Azure Command Documentation</h1>

<h2>Build Docker Image</h2>

<h4>Build a Docker image from a Dockerfile located in the current directory. Tag the image for later use.</h4>

<pre><code>docker build --tag &lt;image_name&gt;:&lt;tag&gt; .
</code></pre>

<h4>Example:</h4>

<pre><code>docker build --tag my-fastapi-app:test12 .
</code></pre>

<h3>Run Docker Container</h3>

<h4>Run a Docker container in detached mode. Map port 80 on the host to port 80 in the container.</h4>

<pre><code>docker run -d --name &lt;container_name&gt; -p 80:80 &lt;image_name&gt;:&lt;tag&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>docker run -d --name my-fastapi-container -p 80:80 my-fastapi-app:test12
</code></pre>

<h2>Azure CLI Commands</h2>

<h3>Login to Azure</h3>

<h4>Authenticate and log in to your Azure account.</h4>

<pre><code>az login
</code></pre>

<h3>Create a Resource Group</h3>

<h4>Create a new resource group in a specified location.</h4>

<pre><code>az group create --location &lt;location&gt; --name &lt;resource_group_name&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>az group create --location eastus --name my-resource-group
</code></pre>

<h3>Delete a Resource Group</h3>

<h4>Delete an existing resource group.</h4>

<pre><code>az group delete --name &lt;resource_group_name&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>az group delete --name my-resource-group
</code></pre>

<h3>Create a Container Registry</h3>

<h4>Create a new container registry within a specified resource group.</h4>

<pre><code>az acr create --resource-group &lt;resource_group_name&gt; --name &lt;registry_name&gt; --sku &lt;sku&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>az acr create --resource-group my-resource-group --name mycontainerregistry --sku Basic
</code></pre>

<h3>Enable Admin Access for Container Registry</h3>

<pre><code>az acr update -n &lt;registry_name&gt; --admin-enabled true
</code></pre>

<h4>Example:</h4>

<pre><code>az acr update -n mycontainerregistry --admin-enabled true
</code></pre>

<h3>Show Container Registry Credentials</h3>

<pre><code>az acr credential show --name &lt;registry_name&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>az acr credential show --name mycontainerregistry
</code></pre>

<h3>Log in to Container Registry</h3>

<pre><code>docker login &lt;registry_url&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>docker login mycontainerregistry.azurecr.io
</code></pre>

<h3>Build and Push Docker Image to Container Registry</h3>

<pre><code>az acr build --platform &lt;platform&gt; -t &lt;registry_url&gt;/&lt;image_name&gt;:&lt;tag&gt; -r &lt;registry_name&gt; .
</code></pre>

<h4>Example:</h4>

<pre><code>az acr build --platform linux/amd64 -t mycontainerregistry.azurecr.io/my-fastapi-app:latest -r mycontainerregistry .
</code></pre>

<h3>Create a Container App Environment</h3>

<pre><code>az containerapp env create --name &lt;environment_name&gt; --resource-group &lt;resource_group_name&gt; --location &lt;location&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>az containerapp env create --name my-fastapi-env --resource-group my-resource-group --location eastus
</code></pre>

<h3>Create a Container App</h3>

<pre><code>az containerapp create --name &lt;app_name&gt; \
    --resource-group &lt;resource_group_name&gt; \
    --image &lt;registry_url&gt;/&lt;image_name&gt;:&lt;tag&gt; \
    --environment &lt;environment_name&gt; \
    --registry-server &lt;registry_url&gt; \
    --registry-username &lt;registry_username&gt; \
    --registry-password &lt;registry_password&gt; \
    --ingress external \
    --target-port 80
</code></pre>

<h4>Example:</h4>

<pre><code>az containerapp create --name my-fastapi-app \
    --resource-group my-resource-group \
    --image mycontainerregistry.azurecr.io/my-fastapi-app:latest \
    --environment my-fastapi-env \
    --registry-server mycontainerregistry.azurecr.io \
    --registry-username mycontainerregistry \
    --registry-password &lt;password_here&gt; \
    --ingress external \
    --target-port 80
</code></pre>

<h3>Update and Redeploy</h3>

<h4>Update Docker Image</h4>

<pre><code>docker build --tag &lt;image_name&gt;:&lt;tag&gt; .
</code></pre>

<h4>Example:</h4>

<pre><code>docker build --tag my-fastapi-app:test12 .
</code></pre>

<h3>Push Updated Image to Container Registry</h3>

<pre><code>az acr build --platform &lt;platform&gt; -t &lt;registry_url&gt;/&lt;image_name&gt;:&lt;tag&gt; -r &lt;registry_name&gt; .
</code></pre>

<h4>Example:</h4>

<pre><code>az acr build --platform linux/amd64 -t mycontainerregistry.azurecr.io/my-fastapi-app:latest -r mycontainerregistry .
</code></pre>

<h3>Redeploy Updated Image</h3>

<pre><code>az containerapp revision copy --name &lt;app_name&gt; --resource-group &lt;resource_group_name&gt; --image &lt;registry_url&gt;/&lt;image_name&gt;:&lt;tag&gt;
</code></pre>

<h4>Example:</h4>

<pre><code>az containerapp revision copy --name my-fastapi-app --resource-group my-resource-group --image mycontainerregistry.azurecr.io/my-fastapi-app:latest
</code></pre>

</body>
</html>
