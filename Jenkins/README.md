![image](https://github.com/user-attachments/assets/7bc871f8-5125-4b71-a254-1d0ee950a234)

### [Установка](https://www.jenkins.io/download/)

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

## Plugins

расширяют функционональные возможности Jenkins.

куча плагинов на серве в Avaliable plugins

и еще столько же на [сайте](https://plugins.jenkins.io/), тут можно выбрать конкретную версию плагина( если нужна не latest) и загрузить на сервер через ![image](https://github.com/user-attachments/assets/71a7769f-4924-424e-b080-222730701027)


# Pipeline

## Freestyle project
создать Item - Freestyle project - Шаги сборки - Shell-script - ```echo "Hello Dota!"```

![image](https://github.com/user-attachments/assets/815104d4-99c5-4240-a2ae-ce6db8f4cc51)

![image](https://github.com/user-attachments/assets/717062ef-c98e-451b-b0e3-038663420a79)

### Publish over ssh
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

## Jenkins slaves/nodes
ssh-agent сохраняет ssh-key и username в credentials, а ssh-slaves добавляет agents через ssh

на слейвах должна быть установлена java
```sudo apt-get install fontconfig openjdk-17-jre```

![image](https://github.com/user-attachments/assets/b2cabfb0-6f51-4340-84cb-ee0d0d952205)

![image](https://github.com/user-attachments/assets/85f9382c-e9a7-44ae-93a0-eb5151038399)

на aws-сервах приватный ключ указываем .pem ключ, который в авс привязан изначально к серву

вместо мастер-сервера - ставим указываем лейблы, чтоб таски бежали на слейвах, по бест-практису мастер должен быть только оркестратором для сборок на слейвах

![image](https://github.com/user-attachments/assets/5916c449-0553-4bad-8b19-e2ddb8fab435)

![image](https://github.com/user-attachments/assets/ad92a659-e4f1-4919-813c-0414b3c5e711)

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


