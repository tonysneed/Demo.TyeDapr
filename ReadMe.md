# Tye with Dapr

**Installation**: https://www.nuget.org/packages/Microsoft.Tye

> **Note**: On Mac you will need to set the tools PATH explicitly for zsh. Create a `.zprofile` file in home directory with:
> `export PATH="$PATH:/$HOME/.dotnet/tools"`

**Frontend and Backend Apps**: https://github.com/dotnet/tye/blob/master/docs/recipes/dapr.md

**Type with Dapr**:
- Run `tye init` at the solution level to create `tye.yml` file
- Under `name` add:

    ```yaml
    extensions:
    - name: dapr
    ```

Run at the solution level: `tye run`

**Tye Dashboard**: http://localhost:8000/
- Click on service bindings to view app or web api.
- Append `swagger` or `weatherforecast` to web api url.

**Debugging with Tye**: https://dev.to/thangchung/debugging-dapr-application-using-tye-tool-1djb
- Run `tye run --debug Backend/Backend.csproj`
- In Visual Studio attach to both **FrontEnd.exe** and **Backend.exe** processes.
- Set breakpoints and debug.

**Kubernetes One-time Setup**: https://github.com/dapr/quickstarts/tree/v1.0.0/hello-kubernetes

- [Enable Kubernetes](https://docs.docker.com/docker-for-windows/#kubernetes) on Docker Desktop.
- Install [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) ([Mac](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/), [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)).

    ```
    kubectl port-forward kubernetes-dashboard-7798c48646-ctrtl 8443:8443 --namespace=kube-system
    ```

- Install Kubernetes **Dashboard**.

    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/alternative/kubernetes-dashboard.yaml
    kubectl proxy
    ```
    - Get kubernetes **access token**.

    ```
    kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"
    ```

    - Copy the access token, open the Kubernetes Dashboard, then paste it. (*Save the token as password in browser when prompted.*) http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy

- Initialize **Dapr for Kubernetes**.

    ```
    dapr init -k
    dapr status -k
    ```

**Kubernetes Deployment with Tye**: https://github.com/dotnet/tye/blob/main/docs/reference/deployment.md

> **Note**: Sign into Docker Desktop to use Docker Hub container registry.

- Deploy with Tye using interactive mode.
    ```
    tye deploy -i
    ```
   - When prompted enter container registry or Docker Hub account name.

- View the services using `kubectl`: `kubectl get service`.

- Apply port forwarding to the frontend service.
    ```
    kubectl port-forward svc/frontend 5000:80
    ```
   - Navigate to http://localhost:5000 to view the frontend on Kubernetes.

- Apply port forwarding to the backend service (optional).
    ```
    kubectl port-forward svc/backend 5050:80
    ```
   - Navigate to http://localhost:5050/weatherforecast to view output for the backend on Kubernetes.

- When finished you can delete the application from Kubernetes.
    ```
    tye undeploy
    ```
