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

Для примера возьму репо на [гитлабе](https://gitlab.com/Wireflex/teamcity.git)

Привязываем TeamCity к GitHub ( Settings - Developer settings - OAuth Apps - TeamCity ) там же и токен берём для тимсити

TeamCity может сам определить шаги для билда ( можно редактировать )

![image](https://github.com/user-attachments/assets/bba02656-dca9-4186-a7d1-9c51a7cad32e)

![image](https://github.com/user-attachments/assets/9cfbab1c-23ae-44ae-b5c4-c83686f5af1f)

![image](https://github.com/user-attachments/assets/45554080-afdc-4872-944d-270a12e61b71)

![image](https://github.com/user-attachments/assets/c6f83084-d3d3-467c-85cc-25ae4acb3228)

связываем с DockerHub в настройках тимсити

![image](https://github.com/user-attachments/assets/359953b7-0246-4099-885b-bb33f989db38)

и в билде так же тыкаем в build features

## Agents

если тимсити-сервер бежит в контейнере, агента нужно запустить с host-сетью и сразу с правами на докер
```
docker run --network host -e SERVER_URL="http://localhost:8111"  -v team_city_agent_config_two:/data/teamcity_agent/conf  -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/bin/docker -d jetbrains/teamcity-agent
```
