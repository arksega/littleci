# littleci

This is a simple example to deploy a CI/CD environment on a kubernetes cluster

```bash
# Deploy gitea
kubectl create -f gitea.yaml
```
Now you need to create an application on gitea so drone can inrect with it, go to http://<you-server>:32642/user/settings/applications and create a new application.
Redirect URI should be http://<your-server>:32020/login. Update `DRONE_GITEA_CLIENT_ID` and `DRONE_GITEA_CLIENT_SECRET` in drone-values.yaml as well as `DRONE_SERVER_HOST` and `DRONE_GITEA_SERVER` with proper values.

```bash
# Deploy drone
helm install drone https://github.com/gtaylor/drone-charts/releases/download/drone-0.1.3/drone-0.1.3.tgz --values drone/drone-values.yaml
helm install drone-runner https://github.com/gtaylor/drone-charts/releases/download/drone-runner-kube-0.1.2/drone-runner-kube-0.1.2.tgz --values drone/drone-runner-values.yaml
```

After that we can use our drone deployment, here is and example to build a hello world application in go, package it and publish the result to gitea when we push a new tag as release mechanism.

```bash
# Clone example repo
git clone https://github.com/arksega/drone_test_repo.git

# Create a new repo in gitea and import
git remote add gitea http://<repo-url>
git push -u gitea master
```

For the publish part to work, gita pluging in drone side needs a token. To create that token go to http://<your-server>:32642/user/settings/applications and create a toke and copy it.

After that go to drone panel and click on `sync`, then `activate` your newly created repo. On settings create a new secret with the name `gitea_api_key` and use your new token as `secret value`, then click on `add a secret`

Now is time for the magic to happen; crea a tag on drone_test_repo and push it to gitea, then take a look to your drone panel.

```bash
# Push last tag
git tag 0.0.1
git push gitea 0.0.1
```
