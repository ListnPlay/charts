# charts

## Usage

#### Setup helm repo
        helm repo add featurefm 'https://raw.githubusercontent.com/listnplay/charts/master/'
        helm repo update

#### Install
        helm install --name druid featurefm/druid -f values.yaml