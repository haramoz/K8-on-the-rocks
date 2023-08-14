# Day 5

## Deploying an python app

Python script: simple.py

#!/usr/bin/python3
import time
import socket

while True:
    host = socket.gethostname()
    date = time.strftime("%Y-%m-%d %H:%M:%S")
    now = str(date)

    f = open("date.out", "a")
    f.write(now + "\n")
    f.write(host + "\n")
    f.close()

    time.sleep(5)


Dockerfile:

FROM docker.io/library/python:3
ADD simple.py /
CMD [ "python", "./simple.py" ]

build:

docker build -t simpleapp .
docker run simpleapp