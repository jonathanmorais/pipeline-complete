FROM jfloff/alpine-python

ADD ./ /app/

# If you have some another python dependencies
RUN pip install --upgrade pip

# install requirements
RUN pip install -r /app/requirements.txt

WORKDIR /app

CMD [ "python", "./src/frontend/frontend.py" ]