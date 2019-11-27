# Charts

## Usage

#### Setup helm to use this repo
        helm repo add featurefm 'https://raw.githubusercontent.com/listnplay/charts/master/'
        helm repo update

#### Install
install package with helm, e.g.:

        helm install --name druid featurefm/druid -f values.yaml
        
   
## Update a chart (upload to the repo)
 see [this blog post](https://blog.softwaremill.com/hosting-helm-private-repository-from-github-ff3fa940d0b7)
 for more details (although we use a public repo).
 
 1. Change the version in Chart.yaml
 2. Package and reindex with:
 
        cd charts
        helm package druid
        helm repo index .
 3. Commit & push change
 4. `helm repo update` to grab latest changes