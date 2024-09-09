![image](https://github.com/user-attachments/assets/0120b1e0-c810-440a-b8e5-9b0f2be8229b)

[Установка](https://www.jetbrains.com/help/teamcity/install-and-start-teamcity-server.html#Preliminaries)

<details> <summary>docker-compose-teamcity.yml</summary>

```
services:
  teamcity-server:
    image: jetbrains/teamcity-server
    container_name: teamcity-server
    ports:
      - "8111:8111"
    volumes:
      - team_city_data:/data/teamcity_server/datadir
      - team_city_logs:/opt/teamcity/logs
    restart: unless-stopped

volumes:
  team_city_data:
  team_city_logs:
```
</details>

## Pipeline

Привязываем TeamCity к GitHub ( Settings - Developer settings - OAuth Apps - TeamCity ) там же и токен берём для тимсити

TeamCity может сам определить шаги для билда ( можно редактировать )

![image](https://github.com/user-attachments/assets/62363888-baa1-46e8-87eb-1222b79af261)

связываем с DockerHub в настройках тимсити

![image](https://github.com/user-attachments/assets/359953b7-0246-4099-885b-bb33f989db38)

и в билде так же тыкаем в build features

![image](https://github.com/user-attachments/assets/a5a4761f-7cb8-4636-9ba8-b77035ead1ff)

![image](https://github.com/user-attachments/assets/d68c6e8f-527a-47c5-8998-6bca315de660)

в Root Project указывает приватный ключ серва, куда будем деплоить

![image](https://github.com/user-attachments/assets/bd64c1e0-5a8e-46ec-8f36-19f09b9d22ef)

![image](https://github.com/user-attachments/assets/cb9b426a-409d-4c68-96d6-9b3cacf854c3)

## Agents

если тимсити-сервер бежит в контейнере, агента нужно запустить с host-сетью(хз почему так работает, мб только с localhost, они же в 1 компоузе, в 1 сети,хрень какая-то) и сразу с правами на докер
```
docker run --network host -e SERVER_URL="http://localhost:8111"  -v team_city_agent_config_two:/data/teamcity_agent/conf  -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/bin/docker -d jetbrains/teamcity-agent
```
ну или компоуз
<details> <summary>docker-compose-teamcity-agent.yml</summary>

```
services:
  teamcity-server:
    image: jetbrains/teamcity-server
    container_name: teamcity-server
    ports:
      - "8111:8111"
    volumes:
      - team_city_data:/data/teamcity_server/datadir
      - team_city_logs:/opt/teamcity/logs
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8111/app/rest/app/server"]

  teamcity-agent:
    image: jetbrains/teamcity-agent
    container_name: teamcity_agent
    network_mode: host
    environment:
      - SERVER_URL=http://localhost:8111
    volumes:
      - team_city_agent_config_two:/data/teamcity_agent/conf
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/local/bin/docker:/usr/local/bin/docker
    restart: unless-stopped
    depends_on:
      - teamcity-server

volumes:
  team_city_data:
  team_city_logs:
  team_city_agent_config_two:
```
</details>

в Agents авторизовываем его

и смотрим чтобы он подходил по критериям в Compatible Agents (у нас докер настроен, всё ок)

