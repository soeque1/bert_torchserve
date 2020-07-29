# bert_torchserve

## Reference
- https://github.com/pytorch/serve
- https://github.com/pytorch/serve/blob/master/docker/README.md
- https://medium.com/analytics-vidhya/deploy-huggingface-s-bert-to-production-with-pytorch-serve-27b068026d18

## Prepare
```sh
git clone https://github.com/pytorch/serve.git
cd serve/docker
DOCKER_BUILDKIT=1 docker build --file Dockerfile.dev -t torchserve:latest .
cd ../..
```

```sh
docker run --user $UID -v $PWD/:/home/model-server torchserve /bin/bash -c "./cvt.sh"
```

## server
```sh
docker run --user $UID -v $PWD/:/home/model-server torchserve /bin/bash -c "mkdir model_store && mv bert.mar model_store && mkdir tmp"
docker run -it --user $UID -p 8894:8080 -p 8895:8081 -v $PWD/:/home/model-server torchserve /bin/bash -c "torchserve --start --ts-config configs/bert.cfg"
```

## client
```sh
curl -X POST http://127.0.0.1:8894/predictions/bert -T test_input.txt
```


