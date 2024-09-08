![image](https://github.com/user-attachments/assets/7bc871f8-5125-4b71-a254-1d0ee950a234)

## [Установка](https://www.jenkins.io/download/)

<details> <summary>docker-compose.yml</summary>

```
services:
  jenkins:
    image: jenkins/jenkins:lts
    ports:
      - "8080:8080"
    volumes:
      - jenkins_home:/var/jenkins_home # существующий volume
    restart: unless-stopped

  ssh-agent:
    image: jenkins/ssh-agent
    restart: unless-stopped

volumes:
  jenkins_home:

```
</details>

Обновить - ![image](https://github.com/user-attachments/assets/ae26556a-d88e-4e4e-9fbb-1ce7788cae64) - здесь высветится новая версия - ПКМ - копируем ссылку - и скачиваем на сервер и меняем в ```/usr/share/jenkins```со старым jenkins.war ( старую версию можно переименовать и оставить, на всякий ) - рестарт ![image](https://github.com/user-attachments/assets/8c9ead29-95af-45ad-b2b8-3e5f6a147eff)

в Докере же просто поменять имедж

# Plugins

расширяют функционональные возможности Jenkins.

куча плагинов на серве в Avaliable plugins

и еще столько же на [сайте](https://plugins.jenkins.io/), тут можно выбрать конкретную версию плагина( если нужна не latest) и загрузить на сервер через ![image](https://github.com/user-attachments/assets/71a7769f-4924-424e-b080-222730701027)

# Simple jobs
по дефолту все джобы бегут от Jenkins-пользователя, домашняя директория ```/var/lib/jenkins```, поэтому сразу даём рут права и убираем пароль в visudo ```jenkins ALL=(ALL) NOPASSWD: ALL```
## Freestyle project
создать Item - Freestyle project - Шаги сборки - Shell-script - ```echo "Hello Dota!"```

![image](https://github.com/user-attachments/assets/815104d4-99c5-4240-a2ae-ce6db8f4cc51)

![image](https://github.com/user-attachments/assets/717062ef-c98e-451b-b0e3-038663420a79)

## Publish over ssh
ставим этот плагин

в настройках дженкинса в System в самом низу в поле Key вставляем приватный ключ jenkins-сервера, задаём параметры 

я на удаленном поднял апаче и поменяю дефолт страничку в /var/www/html ( нужно дать права на выполнение этой директории )

<details> <summary>index.html</summary>

```
cat <<EOF> index.html
<!DOCTYPE html>
<html>
<head>
	<title>be1.ru</title>
</head>
<body>
<p><img src="https://sun9-29.userapi.com/impg/4JQ9Vw0M_V3P4cHNhgQqLBZ9sB_AxVU2q2D4aw/4-121d-8Qvc.jpg?size=604x340&amp;quality=95&amp;sign=9ea8f0065af8c686d8bd0eaa87be3c85&amp;type=album" /></p>

<p>&nbsp;</p>
</body>
</html>
EOF
```
</details>

![image](https://github.com/user-attachments/assets/eb3b5308-4ee9-434d-9fc2-71674a759728)

# Jenkins slaves/nodes
ssh-agent сохраняет ssh-key и username в credentials, а ssh-slaves добавляет agents через ssh

на слейвах должна быть установлена java
```sudo apt-get install fontconfig openjdk-17-jre```

![image](https://github.com/user-attachments/assets/b2cabfb0-6f51-4340-84cb-ee0d0d952205)

![image](https://github.com/user-attachments/assets/85f9382c-e9a7-44ae-93a0-eb5151038399)

на aws-сервах приватный ключ указываем .pem ключ, который в авс привязан изначально к серву

вместо мастер-сервера - ставим указываем лейблы, чтоб таски бежали на слейвах, по бест-практису мастер должен быть только оркестратором для сборок на слейвах

![image](https://github.com/user-attachments/assets/5916c449-0553-4bad-8b19-e2ddb8fab435)

можно по красоте создавать везде пользователя jenkins и от него выполнять джобы
```
sudo useradd -m -s /bin/bash jenkins # создаём дженкинс-юзера
sudo usermod -aG sudo jenkins        # добавляем его в группу судо
sudo passwd -d jenkins               # нафиг пароль
su - jenkins                         # от него уже генерируем ключи

# тип рекомендация, хотя некоторые нужны( опенссх точно )
sudo apt-get update
sudo apt install apt-transport-https
sudo apt-get install openssh-server
sudo apt install ssh
sudo apt install default-jre
sudo apt-get install fontconfig openjdk-17-jre
```

---

![image](https://github.com/user-attachments/assets/ad92a659-e4f1-4919-813c-0414b3c5e711)

---

# GitOps

создаём credentials для того, чтобы связать дженкинс и гитхаб

публичный ключ на гитхаб (Setting - SSH and GPG keys) , приватный в дженкинс ( Username как на гитхаба) ( если credentials не активируютс - нужно самому активировать ключ - к примеру склонировать репозиторий через ssh )

![image](https://github.com/user-attachments/assets/f9666327-30dd-4d09-9737-ecbd516ec144)

## Triggers

- Trigger builds remotely (e.g., from scripts) Выдаст url, при переходе на который запустит джобу
![image](https://github.com/user-attachments/assets/9907e41d-b027-4727-85c4-53b69a92a9ac)

http://localhost:8080/job/first_job/build?token=dota2

- Запустить по окончанию сборки других проектов. Когда закончится выполнение других джобов - запустит эту

![image](https://github.com/user-attachments/assets/d4122953-f1f5-42ed-a236-6bfc38cf312f)

- Запускать периодически. Выполнение по cron-расписанию

![image](https://github.com/user-attachments/assets/005f60df-7f69-44fe-9514-7981128d4408)

- Опрашивать SCM об изменениях. Дженкинкс проверяет SCM (github) и реагирует на изменения

связан с управлением исходным кодом, который настраивали выше в GitOps, и тож самое крон-расписание

![image](https://github.com/user-attachments/assets/56ce00a3-e92d-48c2-8ba1-7d4eff189c61)

- GitHub hook trigger for GITScm polling. Основной триггер с плагина,  

![image](https://github.com/user-attachments/assets/aec099ab-5793-4653-a0f9-70300d9b129d)

![image](https://github.com/user-attachments/assets/3dc0151d-1d03-40f3-8848-13edf70e7fb8)

![image](https://github.com/user-attachments/assets/f2104304-7e8e-4e20-bfa5-a583a0310fe3)

Добавляем webhook на гитхабе

![image](https://github.com/user-attachments/assets/37850fcb-71c9-43cb-b503-4cd7e14f787d)

и теперь при каждом коммите будет запускаться сборка

![image](https://github.com/user-attachments/assets/6e7e5df8-9b53-47a1-9425-e4202aec7fa9)

![image](https://github.com/user-attachments/assets/61386f07-9c02-4d62-8423-0344802fa0d7)

![image](https://github.com/user-attachments/assets/d9b74329-c241-4c0c-b7df-52907810f8e4)

# Pipeline

## Pipeline script
и строчим в самом дженкинсе

![image](https://github.com/user-attachments/assets/25bcc068-0ae8-443b-8545-88086c7dc1ea)

![image](https://github.com/user-attachments/assets/e22d6179-4020-4852-8c3a-c5f6057d3ac2)

## Pipeline from SCM
уже указываем Jenkinsfile в SCM (github)

![image](https://github.com/user-attachments/assets/434923aa-a337-4d2c-967d-3509e34cf4f7)

ну и сам [Jenkinsfile](https://github.com/Wireflex/CI-CD/blob/1587578415216c78c80e93f00e927bf5c1942536/Jenkins/Nginx_html/Jenkinsfile),
[Dockerfile](https://github.com/Wireflex/CI-CD/blob/48cb5f9360dc0011778572baaa29efaae5e087b8/Jenkins/Nginx_html/Dockerfile) и [index.html](https://github.com/Wireflex/CI-CD/blob/ce83955d5e30e6c4e450ef2254b892261ef3484e/Jenkins/Nginx_html/index.html)
