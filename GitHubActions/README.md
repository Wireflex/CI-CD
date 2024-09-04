![image](https://github.com/user-attachments/assets/e3876923-e5f6-4c4a-b87c-203fc8b944e1)

Включение автоматизации происходит добавлением файлов YAML в специальную директорию [.github/workflows](https://github.com/Wireflex/CI-CD/tree/83eb771a3e8e0a00445a0b474f67807fc6815588/GitHubActions/.github/workflows%20)

Либо тыкнуть на ![image](https://github.com/user-attachments/assets/25470e4a-a693-4fd3-98bb-6f02c0c12c35)и выбрать готовый или создать свой ![image](https://github.com/user-attachments/assets/bb082f8b-98bf-4707-8f31-68cf7cc9ea74)

---

Нужно задать *name* и запомнить его, понадобится позже, а так же глобальные переменные *env* (для всего пайплайна) "github.sha" взято из [GitHub Variables](https://docs.github.com/ru/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables), вот эта штука ![image](https://github.com/user-attachments/assets/999e9ffd-9f42-4936-910d-ad624dc8b7df)

```
name: Practice_actions
env:
  APPLICATION_NAME    : "MyFlask"
  DEPLOY_PACKAGE_NAME : "flask-deploy-ver-${{ github.sha }}" # рандом хрень + системная переменная гитхаба
```

При каких событиях будет запускаться пайплайн 
```
on:             # Ctrl + Space для подсказки
  push:         # при пуше
    branches:   # в ветку
      - main    # название
```
Далее задаём *jobs*(таски), можно сразу несколько 
```
jobs:
  my_testing:
    runs-on: ubuntu-latest # этот побежит на раннере гитхаба
...
task for my_testing
...

  my_deploy:
    runs-on: self-host   # а этот на своём раннере(нужно его прикрутить сначала)
    needs: [my_testing]  # запустится после my_testing (по дефолту они одновременно бегут, и деплой может выполнится раньше теста)
...
task for my_deploy
...
```
Далее *steps* в каждой *jobs*
```
...
jobs my_testing
...

    steps:
    - name: Print Hello Message in Testing
      run : echo "Hello World from Testing job"

    - name: Execure few commands
      run : |
        echo "Hello Message1"
        echo "Hello Message2"
        echo "Appication name: ${{ env.APPLICATION_NAME }}"

...
jobs my_deloy
...

    steps:
    - name: Print Hello Message in Deploy
      env:
        LOCAL_VAR: "This is Superr local Environment variable"  # тут уже локальная переменная только для my_deploy
      run : echo "Hello World from Deploy job"
      run : |
        echo "Var1 = ${{ env.VAR1 }}"
        echo "Var2 = ${{ env.VAR2 }}"
        echo "Var3 = $LOCAL_VAR"

    - name: Printing Deployment package
      run : echo "Deploy package name is ${{ env.DEPLOY_PACKAGE_NAME }}"

```
