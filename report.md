# Лабораторная работа 4

1.  **Создание докерфайла**

Создаем в папке ```lab4```  ```Dockerfile```
  ```bash
   nano Dockerfile
   ```

Записываем в него:
  ```bash
   # Устанавливает базовый образ, с которого начинается сборка
   FROM ubuntu:latest
   # Обновляет доступные пакеты и устанавливает libaa-bin и iputils-ping
   RUN apt-get update && apt-get install -y libaa-bin iputils-ping
   # Задаёт команду, которая будет выполняться при запуске контейнера
   CMD ["aafire"]
   ```

2.  **Сборка**

``` bash
docker build -t aafire_container .
```

3.  **Запуск контейнера**

``` bash
docker run -d -it --name aafire1 aafire_container
docker run -d -it --name aafirer2 aafire_container
```
![1](https://github.com/user-attachments/assets/6e9aa6d9-85d0-42fb-a5a1-9b071e24f285)

4. **Создание сети**

 ``` bash
docker network create myNetwork
```

5. **Подключение контейнеров к сети**

``` bash
docker network connect myNet aafire1
docker network connect myNet aafire2
```

6. **Проверка соединения**

   
``` bash
ping -c 5 172.18.0.3

```

![3](https://github.com/user-attachments/assets/39f3603c-0e2c-4436-b45e-8e8cb939b441)
