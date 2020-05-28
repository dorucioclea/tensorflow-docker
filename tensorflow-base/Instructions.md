# Base Tensorflow images

This folder contains artifacts needed to build the base tensorflow images

* Tensorflow CPU (current development version) with Jupyter
* Tensorflow GPU (current development version) with Jupyter

NOTE: These images are needed for any further experiments so please build them first

In order to build the `cpu tensorflow image` (the only one currently used)

```sh
> ./build.cpu.development.sh
```

In order to build the `gpu tensorflow image` (the only one currently used)

```sh
> ./build.gpu.development.sh
```