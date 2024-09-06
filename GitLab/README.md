![image](https://github.com/user-attachments/assets/d06e49ef-a960-47a5-a9df-09bea2642523)

## [GitLab](https://gitlab.com/)

По большому счёту +- как GitHub

Сразу добавляем ssh-ключ ```тык на профиль - edit profile - ssh keys```

## Создание раннера

Внутри проекта - Setting - CI/CD - Runners - New project runner

либо пишем таги, либо ![image](https://github.com/user-attachments/assets/9f1ab826-a0c7-467a-9092-bf2bad94d6d6)

<details> <summary>установка раннера</summary>

``` 
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```
</details>

регистрируем по инструкции и ```gitlab-runner run```

Все джобы побегут от пользователя gitlab-runner, и если нужно выполнить судо-команды, то в visudo нужно дать ему рут-права и убрать пароль ![image](https://github.com/user-attachments/assets/effbe15a-4207-4232-9aa2-18d53a8306fa)

Глобальные переменные и секреты в Проект - Setting - CI/CD - Variables

## Pipeline

создаём ```.gitlab-ci.yml``` в корне репозитория и начинаем писать пайплайн

Перечисляем stages
```
stages:
    - build
    - deploy
```

Задаём любое имя стейджу и расписываем его
```
first_stage:
    stage: build          # из stages: выше
    image: ubuntu:latest  # на чём будет производится данный стейдж
    tags:
        - dota            # этот тэг у раннера, он по нему и подхватит пайплайн
    script:
        - mkdir Dota
        - cd Dota
        - echo "Dota" >> dota.sh
    artifacts:
        paths:
            - Dota/
    allow_failure: true  # продолжит выполнять другие stages, даже если этот зафейлится

testing:
    stage: test
    tags: 
        - test
    script:

deploying:
    stage: deploy
    script:
        - 
    when: manual  # Continuous Delivery, запустить пайплайн можно только вручную
```
сохраненный артефакт ![image](https://github.com/user-attachments/assets/1f87b7da-dc3d-4f8c-baa3-788e374b620b)

Свои билды можно хранить в гитлабе в Проект - Deploy - Container registry
```
docker login registry.gitlab.com
docker build -t registry.gitlab.com/wireflex/dota .
docker push registry.gitlab.com/wireflex/dota
```
![image](https://github.com/user-attachments/assets/8bac9d2f-49b8-407c-b991-b218f79ea41e)
