FROM python:3.7.13-bullseye # We start from a python/debian image.

WORKDIR /backend # Creating and establishing our working directory.

RUN apt update && apt install -y libgraphviz-dev && pip install --no-cache-dir -r requirements.txt && rm -rf /var/lib/apt/lists/* # We make an update, install lib necessary and dependencies and erase cache.

COPY [".", "."] # We copy all the files from our folder to the image.

EXPOSE 8000 # We expose the necessary port

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"] # We deploy our app

