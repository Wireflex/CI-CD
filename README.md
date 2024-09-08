![image](https://github.com/user-attachments/assets/4823a4b8-1f0a-4bc0-8812-3e339efe3a6a)

# Continuous Integration (CI) и Continuous Deployment (CD) 
это практики разработки программного обеспечения, которые помогают командам улучшить качество кода и ускорить процесс доставки программного обеспечения.

## Continuous Integration (CI) интеграция :  
- Это практика, при которой разработчики регулярно вливают код в общий репозиторий. 
- Каждый коммит проходит через автоматизированные тесты и проверки, что позволяет быстро выявлять дефекты и интегрировать изменения.
- CI помогает предотвратить "интеграционные проблемы", которые могут возникнуть, когда разные разработчики работают над одной частью кода.

## Continuous Delivery (CD) доставка : 
- Это прагматичный подход, при котором новые изменения кода всегда готовы к развертыванию в любую момент.
- Однако, в отличии от Continuous Deployment, автоматическое развертывание на продакшн не происходит: это ручной процесс, который требует согласия команды или отдельного лица. 
- Continuous Delivery гарантирует, что программное обеспечение можно развернуть в любое время и оно по-прежнему будет работать.

## Continuous Deployment (CD) развёртывание:
- Это более автоматизированный процесс, который идет на шаг дальше Continuous Delivery. Здесь любое изменение, прошедшее все тесты, автоматически разворачивается в производственной среде.
- В этом подходе нет ручного вмешательства — каждая новая версия программы сразу же становится доступной пользователям.

## Основные этапы CI/CD:

1. Кодирование:
   - Разработчики пишут код и сохраняют его в системе контроля версий (например, Git).

2. Сборка (Build):
   - Код из репозитория автоматически собирается с помощью средств сборки (например, Maven, Gradle).
   - На этом этапе также выполняется компиляция кода и создание артефактов (бинарных файлов, библиотек и т. д.).

3. Тестирование:
   - На этапе тестирования выполняются автоматизированные тесты (unit-тесты, интеграционные тесты).
   - Тесты помогают определить наличие ошибок и дефектов в коде, прежде чем он будет развернут.

4. Интеграция:
   - Успешно протестированный код интегрируется с основной веткой репозитория.
   - Если происходят конфликты или ошибки, разработчики исправляют их.

5. Развертывание (Deployment):
   - Для Continuous Delivery код подготавливается для развертывания на разных средах (например, staging, production).
   - Для Continuous Deployment изменения автоматически развертываются в производственной среде.

6. Мониторинг:
   - После развертывания приложения происходит мониторинг его работы (например, использование ресурсов, журналы ошибок).
   - На этом этапе команда отслеживает производительность и выявляет любые возможные проблемы.

## [Jenkins](https://github.com/Wireflex/CI-CD/blob/7ade2650a8f51ed0a84bf1c25ef5bee920f265f2/Jenkins/README.md)

## [GitLab CI](https://github.com/Wireflex/CI-CD/tree/7ade2650a8f51ed0a84bf1c25ef5bee920f265f2/GitLab)

## [TeamCity](https://github.com/Wireflex/CI-CD/tree/3469a629e1b08a9f3952418724388a901a49a2fb/TeamCity)

## [GitHub Actions ](https://github.com/Wireflex/CI-CD/tree/704827363c45f2fd46b6450152110e679f8ca004/GitHubActions)

[![Practice_actions](https://github.com/Wireflex/CI-CD/actions/workflows/main.yml/badge.svg)](https://github.com/Wireflex/CI-CD/actions/workflows/main.yml)
