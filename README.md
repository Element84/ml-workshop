# Setup

- Launch one EC2 instance for each participant and perform the below steps on each
    - Launch template: `MLWorkshopLaunchTemplate`
- Clone repo and checkout branch
  ```sh
  git clone https://github.com/Element84/ml-workshop.git
  cd ml-workshop && \
  git checkout internal-workshop
  ```
- Install `mamba`
  ```sh
  wget -q -O ~/micromamba.sh https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge-pypy3-Linux-x86_64.sh && \
    chmod +x ~/micromamba.sh && \
    bash ~/micromamba.sh -b -p /home/ubuntu/conda

  export PATH=/home/ubuntu/conda/bin:$PATH
  export LD_LIBRARY_PATH=/home/ubuntu/conda/lib/:$LD_LIBRARY_PATH
  mamba init
  exec $SHELL
  ```
- Create and activate environment
  ```sh
  mamba env create -f env.yml
  mamba activate ml-workshop
  ```
- Install GDAL
  ```sh
  mamba install -y -c conda-forge gdal=3.6.3
  ```
- Run `aws configure` and set credentials and region (`us-east-1`).
- Copy over sandstorm training data
  ```sh
  aws s3 sync s3://ml-workshop-internal/sandstorm/data/ ~/ml-workshop/notebooks/02-sandstorm_case_study/data/training
  ```
- Start JupyterLab server
  ```sh
  jupyter lab password
  ```
  ```sh
  cd ~ && mkdir ssl && cd ssl && openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem && cd -
  cd ml-workshop/notebooks
  jupyter lab --certfile=~/ssl/mycert.pem --keyfile ~/ssl/mykey.key
  ```
- Ask each participant to run the following in their local terminal:
  ```sh
  ssh -i ~/ml-workshop.pem -N -f -L 8888:localhost:8888 ubuntu@abc.amazonaws.com
  ```
- Ask each participant to navigate to https://localhost:8888/ in their local browser.
