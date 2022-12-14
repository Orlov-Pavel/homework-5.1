# Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения."

1. Отличия типов виртуализации:
   * Полная (аппаратная) виртуализация - Гипервизор такого типа сам по себе является ОС и не требует её установки как таковой, предоставляет доступ для виртуальных машин напрямую к железу.
   * Паравиртуализация - Устанавливает на предустановленную ОС и предоставляет доступ для виртуальных машин к ресурсам этой ОС.
   * Виртуализация на основе ОС. Позволяет запускать изолированные виртуальные машины, но не эмулирует ОС внутри гостевой системы, а предоставляет доступ к ядру хоста.
2. Мой выбор:
   Условия использования | Организация серверов
   --------------------- | --------------------
   Высоконагруженная база данных, чувствительная к отказу | Физические сервера. Т.к. БД высоконагружена и чувствительна к отказу логично выделить для неё все аппаратные ресурсы сервера, не используя их для чего либо ещё.
   Различные web-приложения | Виртуализация уровня ОС. Как правило, web-приложениям не требуется слой виртуализации ОС и достаточно использования ядра ОС хоста.
   Windows системы для использования бухгалтерским отделом | Паравиртуализация. Бухгалтерский отдел может зависеть от различного ПО не всегда требовательно к ресурсам, по этому логично использовать паравиртуализацию т.к. можно довольно легко выделить необходимое количество аппаратных ресурсов каждой ВМ и быстро разворачивать новые машины используя шаблоны ВМ.
   Системы, выполняющие высокопроизводительные расчёты на GPU | Можно использовать как физические сервера, так и паравиртуализацию. Если система требует большое количество ресурсов, то логично выделить ей один мощный сервер и не тратить его ресурсы на виртуализацию. Если таких систем несколько, но они не требуют огромного количества ресурсов или использую их не регулярно, то логично использовать паравиртуализацию с возможностью проброса GPU.
3. Системы управления виртуализацией по сценариям:
   1. Выбрал бы Hyper-V т.к. преимущественно Windows based инфраструктура, но способен запускать и основные Linux дистрибутивы, поддерживает Hyper-V реплики, для автоматизированного механизма создания резервных копий можно использовать решение от майкрософт: DPM.
   2. Выбрал бы KVM т.к. он бесплатный, позволяет запустить практически любую ОС в качестве гостевой, а является частью большинства современных ядер Linux.
   3. Выбрал бы Hyper-V т.к. является частью Windows Server, не требует дополнительного лицензирования и максимально совместим с ОС Windows.
   4. Можно использовать докер или LXE контейнеры т.к. они буду использовать ядро хоста и в виду более быстрого и простого создания позволяют проще и быстрее тестировать ПО на различных дистрибутивах Linux не тратя ресрсы на виртуализацию слоя ОС.
4. Основная проблема использования гетерогенной среды виртуализации - увеличившаяся сложность управления средой виртуализации т.к. каждая среда виртуализации требует своих подходов к управлению, администрированию, созданию ВМ, резервному копированию и т.д. Следственно специалист по виртуализации должен на довольно хорошем уровне быть знаком со всем использующимися средами виртуализации, что в свою очередь может привести к увеличению стоимости специалистов т.к. обычно большинство системных администраторов обладают хорошими знаниями только по одной системе виртуализации.  
При отсутствии явной необходимости в гетерогенной среде я бы предпочёл использовать однородную среду виртуализации т.к. ей проще управлять, её легче масштабировать и требуется меньше различных инструментов и подходов для управления средой, что в свою очередь, уменьшет порог входа и требует меньшей квалификации специалистов.