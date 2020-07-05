FROM python:3.7-slim-stretch

# Grab requirements.txt.
ADD ./requirements.txt /tmp/requirements.txt

# Install dependencies
RUN pip install -qr /tmp/requirements.txt

# Add our code
ADD . /opt/webapp/
WORKDIR /opt/webapp

CMD gunicorn -b 0.0.0.0:5000 webapp:app