# helm-minecraft

Instructions on how to run a Minecraft server using Kubernetes.

## Goals

- Run a Minecraft server that anyone can connect to
- Persist the state (world, player data, etc.) across server crashes/reinstalls

## Instructions

1. Decide whether you want to use an EBS or EFS volume
2. Obtain access to an EBS or EFS volume, depending on which one you decided to use
2. Edit the last line of `.helmignore` to the type of volume you are not using (e.g. `templates/ebs-*.yaml` if you are using EFS)
3. Obtain access to a EKS cluster (instructions at http://arun-gupta.github.io/add-iam-user-to-eks/)
4. Install Helm on the EKS cluster (follow instructions at http://arun-gupta.github.io/helm-eks/) 	
5. Download the Helm chart (https://github.com/AdityaGupta1/charts/tree/master/stable/minecraft)
    - This chart has a few important differences from the one at https://github.com/helm/charts/tree/master/stable/minecraft:
        - Custom PV and PVC which point to a pre-created EBS/EFS volume
        - Batch/shell scripts to easily install/uninstall the chart 
6. Set the volume ID in `ebs-pv.yaml` or `efs-pv.yaml` depending on which volume type you are using
7. Run `helm init --history-max 200` to initialize Helm
8. Run `install-server.bat` or `install-server.sh` depending on which OS you are using
9. Find the external IP of the `minecraft-minecraft` service and connect to that IP in Minecraft
![](server-ip.png)
    - Use `kubectl exec -i <pod name> rcon-cli` to access the server console
![](console.png)
10. Enjoy your **epic** Minecraft server

## Troubleshooting

- Try running `kubectl logs <pod name>` or `kubectl describe pod <pod name>` to print out any error messages