![image](https://github.com/user-attachments/assets/e3876923-e5f6-4c4a-b87c-203fc8b944e1)

Включение автоматизации происходит добавлением файлов YAML в специальную директорию [.github/workflows](https://github.com/Wireflex/CI-CD/tree/83eb771a3e8e0a00445a0b474f67807fc6815588/GitHubActions/.github/workflows%20)

Либо тыкнуть на ![image](https://github.com/user-attachments/assets/25470e4a-a693-4fd3-98bb-6f02c0c12c35)и выбрать готовый или создать свой ![image](https://github.com/user-attachments/assets/bb082f8b-98bf-4707-8f31-68cf7cc9ea74)

Нужно задать имя и запомнить его, понадобится позже
```
name: Test_actions
```

При каких событиях будет запускаться пайплайн 
```
on:             # Ctrl + Space для подсказки
  push:         # при пуше
    branches:   # в ветку
      - main    # название
```
