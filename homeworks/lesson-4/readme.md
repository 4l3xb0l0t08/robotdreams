## Домашне завдання №4

#### Посилання на DockerHub:

[https://hub.docker.com/repository/docker/4l3xb0l0t08/my-nginx-image/general](https://hub.docker.com/repository/docker/4l3xb0l0t08/my-nginx-image/general)

### Завантаження зібраних образів 0.1 та 0.2 з DockerHub

#### Покроково

1. Завантажуємо імідж 0.1:

   *sudo docker pull 4l3xb0l0t08/my-nginx-image:0.1*

2. Перевіряємо шо він є у вас в іміджах докера:

   *sudo docker images*

3. Запускаємо контейнер з іміджу 0.1

   *sudo docker run -d -p 8080:80 4l3xb0l0t08/my-nginx-image:0.1*

4. Перевіряємо контейнер на працездатність

  4.1 Процеси - порти:

    *sudo docker ps*

  4.2 Перевіряємо чи добре працює Nginx:

   *curl http://localhost:8080*

Якшо все ок, виконуємо ті само кроки для іміджу 0.2

> Попередньо зупинивши контейнер з іміджа 0.1
> -- *sudo docker stop id контейнера який зупиняємо*


1. Завантажуємо імідж 0.2:

   *sudo docker pull 4l3xb0l0t08/my-nginx-image:0.2*

2. Перевіряємо шо він є у вас в іміджах докера:

   *sudo docker images*

3. Запускаємо контейнер з іміджу 0.2

   *sudo docker run -d -p 8080:80 4l3xb0l0t08/my-nginx-image:0.2*

4. Перевіряємо контейнер на працездатність

  4.1 Процеси - порти:

    *sudo docker ps*

  4.2 Перевіряємо чи добре працює Nginx:

    *curl http://localhost:8080*
