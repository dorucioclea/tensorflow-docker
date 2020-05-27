# Tensorflow models experiment


This experiment attempts to build tensorflow models from scratch allowing us to play with the `object detection API`

Steps to make this work: 

Build base CPU image ([See instructions here](https://github.com/dorucioclea/tensorflow-docker/blob/master/tensorflow-base/Instructions.md))
Build the CPU Tensorflow models image (this might take a while)

```sh
> docker-compose up --build
```

After that you can access the jupyter notebook running at `http://localhost:8888` and use password `root` to enter the notebook

Run `object_detection_tutorial.ipynb` by:

* Go to the menu, Click Cell > Run All
* Wait for the notebook to finish
* After it's done you should see the test images marked with the model's inference

