FROM python:3.7.13-bullseye

WORKDIR /backend

COPY [".", "."]

RUN apt update

RUN apt install -y libgraphviz-dev

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]

