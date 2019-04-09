# DockBoard - Docker Container Management Platform

DockBoard is a Docker Container Management Platform using the django framework and python language and it is suitable for internal network deployment.

### Features

- WEB MONITOR

Concise, efficient, data visualization

- Container MANAGEMENT

Efficient, responsive, second-level deployment

- ANALYTICS

Data visualization, dynamic updates

- DEPLOY SERVICES

Multiple mirroring, load balancing, high availability

### Development Test Environment

- Python 3.6 (Recommend)
- Django 2.0 (Necessary)
- Docker 18.03-ce
- Redis 2.0.6

### Third party plugins (Mandatory)

- django-bootstrap3
- psutil
- docker-py
- celery
  > Install plugins:

```shell
pip install -r requirements.txt
```

### Running the app in Docker

```bash
# Create docker network
docker network create dcmp
# Create redis as message queue
docker run -d --name dcmp-redis --net dcmp  redis
# Run dcmp backend
docker run -itd --name dcmp-backend \
       -v /var/run/docker.sock:/var/run/docker.sock \
       --net dcmp  \
       registry.cn-hangzhou.aliyuncs.com/geekcloud/dcmp:backend
# Run DCMP frontend
docker run -itd --name dcmp-nginx \
       -p 8000:80 \
       --net dcmp \
       registry.cn-hangzhou.aliyuncs.com/geekcloud/dcmp:nginx
```

> You can see the app: http://localhost:8000/
>
> username:admin password:dcmpdcmp123

### Usage

- Initialize Docker (PreStep):

```shell
docker swarm init #Please Your make sure your Docker engine is turned on
```

- Refresh & Synchronize the database(Step 1):

```shell
python manage.py makemigrations
python manage.py migrate
```

- Create Superuser(Step 2):

```shell
python manage.py createsuperuser
```

> Superuser has the ability to create user.

- Run the website(Step 3):

```shell
python manage.py runserver
```

- Start the Redis server(Step 4):

```shell
docker run --name dcmp-redis -p 6379:6379 redis
```

- Start the Celery Worker(Step 5):

```shell
celery -A DCMP worker -l info
```
