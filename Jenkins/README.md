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
      - jenkins_home:/var/jenkins_home
  ssh-agent:
    image: jenkins/ssh-agent
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

