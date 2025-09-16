# Лабораторная работа №1 по DevOps "Nginx" (обычная)

## Задача

Настроить nginx согласно тз (дальнейшие подробности смотреть [здесь](https://docs.google.com/document/d/1jJaKob932N1-91rPy8G_XkyAtOArJe8MCoyJh9iA7lE/edit?tab=t.0)).

## В процессе работы..

### Установка

Пожалуй, это была самая лёгкая часть данного задания. Тем не менее, ей предшествовала настройка wsl, а также некоторое время и огромные моральные усилия, чтобы принять тот факт, что, увы, снова придётся взаимодействовать с linux.

```bash
    sudo apt update
    sudo apt install nginx -y
    nginx -v
    sudo systemctl start nginx
    sudo systemctl enable nginx
    sudo systemctl status nginx
```

### Начало, середина и конец

Пропустим момент создания прообного сайта, на котором было опробовано взаимодействие с nginx.
