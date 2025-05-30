# Kezia Salsalina Agtyra Sebayang - 2306172086

## Reflection on Hello Minikube

1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?

    When I first port-forwarded directly into the pod, I only ever saw the two startup messages:
    ![alt text](img/image.png)

    After I created a Service for the deployment, everything changed. Every time I hit the app’s URL, a new log line appeared:
    ![alt text](img/image-1.png)
    Reloading twice gave me two GET / lines, three reloads gave three, and so on.

2. What is the purpose of the -n option and why did the output not list the pods/services that you
explicitly created?

    ![alt text](img/image-2.png)
    The -n flag lets me tell kubectl exactly which namespace to query instead of using the default. By default (no -n), Kubernetes looks in the “default” namespace where I deployed my own pods and services—so that’s where my resources show up. When I ran kubectl get pods -n kube-system, I was pointing at the system namespace, which only contains Kubernetes’ built-in components.

## Reflection on Rolling Update & Kubernetes Manifest File

1. Difference between RollingUpdate and Recreate
- RollingUpdate (the default) replaces Pods gradually: it brings up new Pods before taking down old ones, so it can achieve zero-downtime if your app supports it.
- Recreate tears down all existing Pods first, then spins up the new ones. Simpler, but it will have a downtime window while the old Pods are gone.

2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document
your attempt. 

    To exercise the Recreate strategy, I applied the deployment-recreate.yml manifest which specifies strategy.type: Recreate. First, I ran kubectl apply -f deployment-recreate.yml, and Kubernetes acknowledged the change with “deployment.apps/spring-petclinic-rest configured.” Since merely applying the unchanged image doesn’t trigger a rollout, I then issued kubectl rollout restart deployment/spring-petclinic-rest to force a new deployment cycle.

3. Manifest files for executing Recreate deployment strategy.
    deployment-recreate.yml has been committed and pushed.

4.  Benefits of using Kubernetes manifest files

    Using Kubernetes manifest files provides clear advantages. Deployments become repeatable and consistent because every configuration detail is stored as code and versioned in Git. Manifests also streamline complex setups by letting you apply all resources at once with a single command, instead of remembering multiple kubectl flags. In practice, working from YAML feels more organized and reduces the risk of errors caused by mistyped commands. Keeping those files in a repository further enhances collaboration and auditing, since every change is tracked and reviewable.