# Base image called as base here. I'm just assuming this program works with 3.12 because you need to get this information from the developer.
# Usually Python applications are backward compatible, so they should be executable.
FROM python:3.12-slim-bookworm AS base

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN pip install --upgrade pip
RUN pip install -r requirements.txt

COPY . .

RUN opentelemetry-bootstrap -a install 

ENV RECOMMENDATION_PORT 1010

#In Python you don't have to create the binary. You can directly execute the program.
ENTRYPOINT ["python", "recommendation_server.py"]
