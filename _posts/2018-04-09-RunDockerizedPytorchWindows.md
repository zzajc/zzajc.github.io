---
layout: post
title: "Run Dockerized pytorch on Windows"
date: 2018-04-09
tags: docker pytorch beginner
author: Jiri Hanousek
---
In the [last post about dockerized pytorch for windows]({% post_url 2018-03-27-DockerizedPytorchWindows %}) I forgot to include one **very important** beginner information.
How to run it? 

There are many questions that arise when using docker for machine learning. Most importantly
- how to tell python to read my up-to-date script
- where to store tons of datasets

The answer is simple :) Store both in folder which will be mounted to docker container.

On Windows there is one **gotcha!** The folder to be mounted **must** be placed in User folder. No links (symbolic) are followed! I tested every linking method I know of.

First create folder like this
```Shell
'C:\Users\Jiri\dockerMount'
```

Then start interactive session of container and with the folder mounted to /hostMount or any other place you want.

```Shell
docker run -it --mount type=bind,source="$(pwd)"/dockerMount,target=/hostMount pytorch
```

In the container you will need to change directory to the mount

```Shell
cd /hostMount
```

After that you can run your python code. For example:

```Shell
python main.py --model condensenet -b 16 -j 4 mnist --stages 5-5-5 --growth 4-8-16 --gpu 0 -p 10 --epochs 10
```

Pytorch automatically downloads datasets and stores them in the /hostMount. You can also edit the code under Windows and start the training proces
any time you want.

Sure this is far from ideal. However it serves well when you want to do more machine learning than system administration. Edit in Windows run on linux in docker.