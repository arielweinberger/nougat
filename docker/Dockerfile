FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04
# replace CUDA version to your CUDA version.
# You can check your CUDA version with below.
# nvcc -V

RUN apt-get update
RUN apt-get install -y python3
RUN apt-get -y install python3-pip git

# python-multipart was missing which caused the docker to exit soon after start
RUN pip3 install python-multipart
RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
# replace CUDA version to your CUDA version.

RUN mkdir workspace
WORKDIR /workspace

RUN pip3 install fastapi uvicorn[standard] fsspec[http]==2023.1.0
RUN git clone https://github.com/facebookresearch/nougat.git
WORKDIR /workspace/nougat

# The following upgrade for numpy and fsspec was required
RUN pip3 install --no-cache-dir --upgrade numpy
RUN pip3 install --no-cache-dir --upgrade "fsspec[http]>=2023.1.0,<=2023.10.0"
RUN pip3 install --no-cache-dir --upgrade 'typing-extensions>=4.9.0'

RUN python3 setup.py install

RUN pip3 install "transformers==4.38.2"

EXPOSE 8503

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8503"]
# Run this using 'docker run -it -d -p <YOUR PORT>:8503 --gpus all <IMAGE NAME>
