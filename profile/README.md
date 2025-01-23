These instructions are for setting up PROF on a local devnet using our implementation artifacts.
For reproducing analysis from the paper, go to : https://github.com/prof-anon/analysis

## Tools
## Docker 
1. Install docker (https://docs.docker.com/get-started/get-docker/ or https://docs.docker.com/engine/install/)
2. `sudo groupadd docker` 
3. `sudo usermod -aG docker $USER`
4. `newgrp docker`

## Kurtosis
Install kurtosis (version 0.87.2):	
1. `echo "deb [trusted=yes] https://apt.fury.io/kurtosis-tech/ /" | sudo tee /etc/apt/sources.list.d/kurtosis.list`
2. `sudo apt update`
3. `sudo apt install kurtosis-cli=0.87.2 -V`
4. add autocomplete for kurtosis (optional):
		https://docs.kurtosis.com/guides/adding-command-line-completion
5. `kurtosis analytics disable`


## Build PROF artifacts

1. Build the prof bundle merger:
    
    `cd builder`

	`docker build . -t prof-project/builder-prof-merger`
	
    `cd ..`

2. Build the prof sequencer
	
    `cd prof-sequencer`
	
    `docker build . -t prof-project/prof-sequencer`
	
    `cd ..`

## Run PROF

1. First ensure that the prof-relay code is located at ethereum-package/prof-relay  (i.e., inside the ethereum-package)
the dir structure should look like this:
	
    main dir
	- ethereum-package/
	- - prof-relay/
	- builder/
	- prof-sequencer/

2. Start up everything using Kurtosis
	
    `cd ethereum-package`
	
    `kurtosis run --enclave prof ./ --args-file network_params.yaml`

3. Check the logs
	
    `kurtosis service logs prof mev-relay-api`

4. kill the enclave 
	
    `kurtosis enclave rm -f prof`
	
NOTE : use these exact names `builder-prof-merger`, `prof-sequencer` and `prof-relay` for the docker images as that's what the above `network_params.yaml` looks for.