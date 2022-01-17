
# Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения."


---

## Задача 1

Опишите кратко, как вы поняли: в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.

* Полная или аппаратная виртуализация сразу может использовать аппаратные ресурсы сервера и не нуждаетя в операционной системе для управления виртуальными машинами.  
* Паравиртуализация - программное обеспечение, использующее операционную систему для разделения ресурсов между виртуальными машинами.  
* Виртуализация на основе ОС позволяет запускать изолированные "контейнеры" на хосте, которые по сути являются небольшими отдельными копиями хостовой ОС. (Наверное неправильно выразился, но в целом - на linux запускаем только такие же linux гостевые ОС). Удобно для тестирования и прочего создания множества подобных систем.

## Задача 2

Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:
- физические сервера,
- паравиртуализация,
- виртуализация уровня ОС.

Условия использования:
- Высоконагруженная база данных, чувствительная к отказу.
- Различные web-приложения.
- Windows системы для использования бухгалтерским отделом.
- Системы, выполняющие высокопроизводительные расчеты на GPU.

Опишите, почему вы выбрали к каждому целевому использованию такую организацию.


* Для высоконагруженных баз лучшим решением будет аппаратная виртуализация типа ESXI или XEN поскольку при этом максимум ресурсов можно выделить для ВМ и при этом обеспечить отказоустойчивость и резевное копирование.  

* Для различных web-приложений лучше всего подойдет контейнеризация типа Docker или LXC, поскольку их гораздо легче масштабировать в заисимости от нагрузки сервисов.  

* Windows системы для использования бухгалтерским отделом я бы разместил на паравиртуальном Hyper-V к примеру ввиду быстроты и простоты развертывания и обслуживания.  

* Системы, выполняющие высокопроизводительные расчеты на GPU лучше размещать на аппаратной виртуализации для того чтобы задействовать максимум ресурсов.  


## Задача 3

Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:

1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
2. Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux. 

Ответы:

1. ESXI или Vsphere, в заисимости от бюджета :) Машин довольно много и управлять их резервным копированием, миграцией и ресурсами будет удобно. Более того к гостевым системам нет вообще никаких ограничений.
2. Это был бы KVM с Proxmox. Если надо производительно и бесплатно, то это лучший вариант по моему мнению. 
3. Думаю это был бы Free Windows Hyper-V Server ввиду того что управлять им не сильно сложно, эта система имеетмного вариантов бэкапа и не требовательна к железу. Однако гостевые системы Windows всё равно нужно будет лицензировать.  
4. Лучше всего подойдет Docker или LXC, позволяющий довольно просто создавать контейнеры для тестирования с использованием автоматизации.


## Задача 4

Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.

  В управлении несколькими системами виртуализации первая сложность это само управление. Различные сущности требуют различных методов управления - разные консоли и даже различные Операционные системы для этого. Вторая сложность - физические ресурсы будут скорее всего разделены и потребуют различных методов организации серверов, стоек и прочего. Третья сложность это различные навыки в обслуживании различных систем виртуализации и подхода к аппаратным требованиям. 
Разные среды имеют и свои преимущества - какие-то проще и легче в освоении, быстрее в развертывании. Другие сложнее но надежнее и выгоднее финансово. Думаю гетерогенные системы имеют право на существование, тем более что примеров этому куча )   

  Сам бы я стремился к однородной среде, которую легче масштабировать, обновлять и управлять из единого центра. Например ESXI очень удобен в плане единого подхода к управлению, интерфейс мало изменяется годами а систему просто развернуть и мониторить удаленно. Однако в результате слияния нескольких организаций в одну получение целого зоопарка серверов на Hyper-V и proxmox выявило их преимущества в отдельных целевых направлениях использования. 

