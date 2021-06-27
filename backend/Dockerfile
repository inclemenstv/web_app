FROM python:3.8.5-alpine

RUN apk add gcc musl-dev python3-dev libffi-dev openssl-dev

# We copy just the requirements.txt first to leverage Docker cache
COPY ./requirements.txt /app/requirements.txt
WORKDIR /app

RUN pip install -r requirements.txt

COPY . /app

EXPOSE 5000

ENTRYPOINT [ "python" ]
CMD [ "main.py" ]