FROM python:3.10

RUN useradd -m biopb
ENV HOME=/home/biopb

USER biopb
WORKDIR $HOME

ENV PATH="$PATH:$HOME/.local/bin/"

RUN wget https://huggingface.co/jiyuuchc/lacss3-base/resolve/main/lacss3-base?download=true -O lacss3-base

ARG VERSION=0.15.0

RUN pip install --no-cache-dir lacss==$VERSION

RUN pip uninstall -y opencv-python && pip install --no-cache-dir opencv-python-headless

RUN mkdir jax_cache

# copy rest of the code
COPY . .

EXPOSE 50051

ENV JAX_COMPILATION_CACHE_DIR="./jax_cache"
ENV XLA_PYTHON_CLIENT_PREALLOCATE=false
#ENV XLA_PYTHON_CLIENT_MEM_FRACTION

ENTRYPOINT ["python", "-m", "lacss.deploy.remote_server"]

CMD ["--no-token", "--f16", "--modelpath", "./lacss3-base"]
