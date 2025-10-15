## Changing the Application Logo

This guide explains how to update the application logo and related images for your deployment. Instructions are provided for both Kubernetes and Virtual Machine (VM) environments.

---


### Preparing Custom Logo Images

Before proceeding with the deployment steps, ensure you have prepared your custom versions of the following images:

- [App Logo](https://ren3.ai/images/app_logo.svg) → `app_logo.svg`
- [Product Logo](https://ren3.ai/assets/images/img_reentry.png) → `productlogo.png`
- [Favicon](https://ren3.ai/favicon.ico) → `favicon.ico`
- [Logo192](https://ren3.ai/logo192.png) → `logo192.png`

Download these ReN3 images for reference. These files will be required for both Kubernetes and Virtual Machine (VM) deployment methods outlined below.

---

### For Kubernetes Deployments

#### Step 1: Create ConfigMaps for Logo Images

First, create individual ConfigMaps for each image you wish to update. Replace `/local/path/to/images/` with the directory containing your image files.

```bash
# App logo
kubectl create configmap ren3-app-logo \
  --from-file=app_logo.svg=/local/path/to/images/app_logo.svg

# Product logo
kubectl create configmap ren3-web-productlogo \
  --from-file=productlogo.png=/local/path/to/images/productlogo.png

# Favicon
kubectl create configmap ren3-web-favicon \
  --from-file=favicon.ico=/local/path/to/images/favicon.ico

# Logo192
kubectl create configmap ren3-web-logo192 \
  --from-file=logo192.png=/local/path/to/images/logo192.png
```

Verify the ConfigMaps:

```bash
kubectl get configmaps
kubectl describe configmap ren3-app-logo
```

#### Step 2: Update the Web Deployment YAML

Modify your Kubernetes Deployment YAML to mount these ConfigMaps as volumes in the web container. Ensure the `mountPath` matches the application's expected file locations:

```yaml
volumeMounts:
  - mountPath: /usr/app/build/images/app_logo.svg
    name: applogo
    subPath: app_logo.svg
  - mountPath: /usr/app/build/assets/images/img_reentry.png
    name: productlogo
    readOnly: true
    subPath: productlogo.png
  - mountPath: /usr/app/build/favicon.ico
    name: favicon
    readOnly: true
    subPath: favicon.ico
  - mountPath: /usr/app/build/logo192.png
    name: logo192
    readOnly: true
    subPath: logo192.png

volumes:
  - configMap:
      defaultMode: 420
      name: ren3-app-logo
    name: applogo
  - configMap:
      defaultMode: 420
      name: ren3-web-productlogo
    name: productlogo
  - configMap:
      defaultMode: 420
      name: ren3-web-favicon
    name: favicon
  - configMap:
      defaultMode: 420
      name: ren3-web-logo192
    name: logo192
```

#### Step 3: Apply the Changes

Deploy the updated configuration:

```bash
kubectl apply -f <deployment-file>.yaml
```

---

### For Virtual Machine (VM) Deployments

To update the logos in a VM environment, replace the corresponding files in the following locations:

- `/opt/rn3bp/web/build/images/app_logo.svg`
- `/opt/rn3bp/web/build/assets/images/img_reentry.png`
- `/opt/rn3bp/web/build/favicon.ico`
- `/opt/rn3bp/web/build/logo192.png`

After replacing the files, restart the web service.