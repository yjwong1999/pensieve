# Pensieve 
## (Reimplementation for Ubuntu 22 + Python 3 + TF 2.13.0)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1yONkb2k5UCdT3clVUvq3jnT15oOTy0IP?usp=sharing)

Pensieve is a system that generates adaptive bitrate algorithms using reinforcement learning.
http://web.mit.edu/pensieve/

### Prerequisites
- Clone the repo
```
git clone https://github.com/yjwong1999/pensieve.git
```

- Setup Conda environment
```
conda create --name pensieve python=3.8.10
conda activate pensieve 
```

- Install prerequisites (tested with Ubuntu 16.04, Tensorflow v1.1.0, TFLearn v0.3.1 and Selenium v2.39.0)
```
cd pensieve
python setup.py
```

### Get data
- Download traces: put training data in `sim/cooked_traces` and testing data in `sim/cooked_test_traces`
```
# https://github.com/hongzimao/pensieve/issues/134#issuecomment-894000647
cd sim
wget https://github.com/hongzimao/pensieve/files/6943078/cooked_traces.zip
unzip cooked_traces.zip
cd ../
```

### Training
- To train a new model, run:
```
cd sim
python get_video_sizes.py
python multi_agent.py
cd ../
```

The reward signal and meta-setting of video can be modified in `multi_agent.py` and `env.py`. More details can be found in `sim/README.md`.

### Testing
- To test the trained model in simulated environment, first copy over the model to `test/models` and modify the `NN_MODEL` field of `test/rl_no_training.py` , and then in `test/` run `python get_video_sizes.py` and then run 
```
cd test
python get_video_sizes.py
python rl_no_training.py
cd ../
```

Similar testing can be performed for buffer-based approach (`bb.py`), mpc (`mpc.py`) and offline-optimal (`dp.cc`) in simulations. More details can be found in `test/README.md`.

### Running experiments over Mahimahi
- To run experiments over mahimahi emulated network, first copy over the trained model to `rl_server/results` and modify the `NN_MODEL` filed of `rl_server/rl_server_no_training.py`, and then in `run_exp/` run
```
python run_all_traces.py
```
This script will run all schemes (buffer-based, rate-based, Festive, BOLA, fastMPC, robustMPC and Pensieve) over all network traces stored in `cooked_traces/`. The results will be saved to `run_exp/results` folder. More details can be found in `run_exp/README.md`.

### Real-world experiments
- To run real-world experiments, first setup a server (`setup.py` automatically installs an apache server and put needed files in `/var/www/html`). Then, copy over the trained model to `rl_server/results` and modify the `NN_MODEL` filed of `rl_server/rl_server_no_training.py`. Next, modify the `url` field in `real_exp/run_video.py` to the server url. Finally, in `real_exp/` run
```
python run_exp.py
```
The results will be saved to `real_exp/results` folder. More details can be found in `real_exp/README.md`.


## Acknowledgement

1. [Original implementation of pensieve](https://github.com/hongzimao/pensieve)
