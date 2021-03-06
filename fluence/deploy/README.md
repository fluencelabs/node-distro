# How to deploy Fluence
1. Edit deployment_config.json to your needs (explanations: TBD)
2. Install docker: `fab install_docker`
3. Edit `fluence.yml` and `fluence_bootstrap.yml` to your needs
4. Deploy fluence: `fab deploy_fluence`
5. If you need https, deploy caddy: `fab deploy_caddy`
6. If you need slack notifications about containers state, deploy watchdog: `fab deploy_watchdog`

# Fluence deployment scripts and configs
`deployment_config.json` – contains list of IPs to use for deployment
`fab deploy_fluence` – deploys fluence, mediated by `fluence.yml` and `fluence_bootstrap.yml`
`fab install_docker` – installs docker and docker-compose (+ haveged)
`fab deploy_watchdog` – deploys a watchdog to monitor containers (change `SECRET` to desired webhook URL)
`fab deploy_caddy` – deploys Caddy 2.0, configured in code

# Prometheus
`/prometheus` contains basic configuration file, HTML consoles are TBD

# How to deploy Fluence with docker
1. Edit `fluence.yml` and `fluence_bootstrap.yml` to your needs
2. Build image: `docker build -t deploy .`
3. Run `docker run -v $HOME/.ssh:/root/.ssh:ro deploy deploy_fluence` (you can use `deploy_caddy` or `deploy_watchdog` instead of `deploy_fluence` as well)

# macOS without docker
If you're on macOS, and want to avoid using Docker, then this section is for you.

You will need to use `pyenv` to access Python 2.

1. `brew install pyenv openssl@1.1`
2. `pyenv install 2.7.18`
3. `pyenv local 2.7.18`
3. `pyenv exec pip2 install -r requirements.txt --global-option=build_ext --global-option="-L/opt/homebrew/opt/openssl@1.1/lib" --global-option="-I/opt/homebrew/opt/openssl@1.1/include"`
4. `pyenv exec fab deploy_fluence`
