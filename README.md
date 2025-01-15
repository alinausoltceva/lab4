# Лабораторная работа 4  
Для решения понадобится установить Docker. Можно это делать как в виртуальной машине с установленным Linux, так и через WSL (главное на линукс подобной системе).

## Решение задания 
Создаем файл Dockerfile с помощью команды touch или nano. Заходим в этот файл с помощью команды gedit Dockerfile.
Внутри Dockerfile мы пишем:
```
FROM ubuntu:latest  
  
RUN apt-get update && apt-get install -y libaa-bin inetutils-ping && apt-get clean && rm -rf /var/lib/apt/lists/*  

CMD ["aafire"]
```
Мы указали образ, на основе которого все будет работать. Далее указываем, что мы хотим запустить, а именно мы обновляем пакетный менеджер и устанавливаем ПО под названием libaa-bin.
Dockerfile готов, сохраняем и запускаем команду сборки образа с тегом my-aafire.

![Снимок экрана 2024-12-23 164632](https://github.com/user-attachments/assets/2ea0c284-a5a9-4f4f-8e39-9719c345009b)
![Снимок экрана 2024-12-23 164659](https://github.com/user-attachments/assets/aa27f928-8582-4fd6-a50a-5d75429741b5)

Проверим успешность сборки:  

![Снимок экрана 2024-12-23 171737](https://github.com/user-attachments/assets/65ca31d6-a911-41c9-889c-8defef2069b4)

Далее создаем контейнеры:  
```
sudo docker run -it --name cont_one my-aafire
sudo docker run -it --name cont_two my-aafire
```
photo_2024-12-05_14-36-19
Если мы хотим еще раз запустить эти контейнеры то пишем команду sudo docker start cont_one, если же еще мы хотим увидеть повторно огонь, то прописываем еще sudo docker exec -it cont_one aafire.

![Снимок экрана 2024-12-24 161158](https://github.com/user-attachments/assets/5db5d3cc-f250-4d62-842b-058d70bb63e1)


Чтобы создать сеть между контейнерами, нужно запустить оба контейнера с aafire и оставить их в рабочем состоянии. Далее создаем 3-е окно в терминале и создаем сеть с помощью команды:
```docker network create myNetwork```  

После этого подключаем контейнеры к только что созданной сети myNetwork:
```
docker network connect myNetwork my-aafire-1
docker network connect myNetwork my-aafire-2
```
После этого нужно будет подключить контейнеры к вашей сети. Названия контейнеров можно увидеть при выводе списка действующих контейнеров у вас на машине.
```
docker network connect myNetwork mycontainer1
docker network connect myNetwork mycontainer2
```
При помощи команды ping мы проверяем подключение к сетям

```
docker exec -it my-aafire-1 ping my-aafire-2
docker exec -it my-aafire-2 ping my-aafire-1 
```

![image](https://github.com/user-attachments/assets/1fc4269f-5398-4f03-9165-cf316e6bf7c8)

