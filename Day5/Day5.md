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

to find out the output file: (important)
sudo find / -name date.out

sudo tail /var/lib/docker/overlay2/57ceced953abca054b768804f21c8d90ef2c44eaf4776353e9004615269e2e0f/diff/date.out