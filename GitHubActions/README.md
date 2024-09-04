![image](https://github.com/user-attachments/assets/e3876923-e5f6-4c4a-b87c-203fc8b944e1)

Включение автоматизации происходит добавлением файлов YAML в специальную директорию [.github/workflows](https://github.com/Wireflex/CI-CD/tree/83eb771a3e8e0a00445a0b474f67807fc6815588/GitHubActions/.github/workflows%20) (именно в корень репозитория, внутри директории не сработает)

Либо тыкнуть на ![image](https://github.com/user-attachments/assets/25470e4a-a693-4fd3-98bb-6f02c0c12c35)и выбрать готовый или создать свой ![image](https://github.com/user-attachments/assets/bb082f8b-98bf-4707-8f31-68cf7cc9ea74)

---

Нужно задать *name* и запомнить его, понадобится позже,  "github.sha" взято из [GitHub Variables](https://docs.github.com/ru/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables), вот эта штука ![image](https://github.com/user-attachments/assets/999e9ffd-9f42-4936-910d-ad624dc8b7df)

```
name: Practice_actions
env:                # глобальные переменные (для всего пайплайна)
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
    env:           # переменные только для my_deploy
      VAR1 : "This is Job Level Variable1"
      VAR2 : "This is Job Level Variable2"
...
task for my_deploy
...
```
Далее *steps* в каждой *jobs*, идиотские uses(модули) можно найти на [Marketplace](https://github.com/marketplace)
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

    - name: List current folder
      run : ls -la               # нифига не выведет, потому что ничего нет
   
    - name: Git clone my repo
      uses: actions/checkout@v1  # типо клонируем репозиторий, чтоб он отображался
  
    - name: List current folder
      run : ls -la               # и вот теперь уже выводит все файлы в этом репозитории

...
jobs my_deloy
...

    steps:
    - name: Print Hello Message in Deploy
      env:
        LOCAL_VAR: "This is Superr local Environment variable"  # переменная только для "name: Print Hello Message in Deploy"
      run : echo "Hello World from Deploy job"
      run : |
        echo "Var1 = ${{ env.VAR1 }}"
        echo "Var2 = ${{ env.VAR2 }}"
        echo "Var3 = $LOCAL_VAR"

    - name: Printing Deployment package
      run : echo "Deploy package name is ${{ env.DEPLOY_PACKAGE_NAME }}"

    - name: Printing Deployment package
      run : echo "Deploy package name is ${{ env.DEPLOY_PACKAGE_NAME }}"
    
    - name: Lets test some packages if they are here 1
      run : aws --version

    - name: Lets test some packages if they are here 2
      run : zip --version
```
self-host runner устанавливается тут

![image](https://github.com/user-attachments/assets/163561f2-6ce3-4496-94ac-3fec8004a53e)

```./run.sh``` для обычного запуска 

для запуска в качестве службы (на фоне)
```
sudo ./svc.sh install
sudo ./svc.sh start
```
<details> <summary>FINAL-VERSION</summary>

```
name: Practice_actions
env:
  APPLICATION_NAME    : "MyFlask"
  DEPLOY_PACKAGE_NAME : "flask-deploy-ver-${{ github.sha }}"

on: 
  push:
    branches: 
      - main

jobs:
  my_testing:
    runs-on: ubuntu-latest

    steps:
    - name: Print Hello Message in Testing
      run : echo "Hello World from Testing job"
    
    - name: Execure few commands
      run : |
        echo "Hello Message1"
        echo "Hello Message2"
        echo "Appication name: ${{ env.APPLICATION_NAME }}"
    
    - name: List current folder
      run : ls -la
   
    - name: Git clone my repo
      uses: actions/checkout@v1   
  
    - name: List current folder
      run : ls -la
  
  my_deploy:
    runs-on: self-hosted
    needs: [my_testing]
    env:
      VAR1 : "This is Job Level Variable1"
      VAR2 : "This is Job Level Variable2"
    
    steps:
    - name: Print Hello Message in Deploy
      run : echo "Hello World from Deploy job"
      
    - name: Print env vars
      run : |
        echo "Var1 = ${{ env.VAR1 }}"
        echo "Var2 = ${{ env.VAR2 }}"
        echo "Var3 = $LOCAL_VAR"
      env:
        LOCAL_VAR: "This is Superr local Environment variable"
    
    - name: Printing Deployment package
      run : echo "Deplyo pakcage name is ${{ env.DEPLOY_PACKAGE_NAME }}"
      
    - name: List current folder
      run : ls -la
```
</details>

Secrets тут:

![image](https://github.com/user-attachments/assets/a65f13f2-7f9d-4a18-ad59-00241e19d3f6)

