
# Ansible FAQ

### 1. Разница между модулями raw, command и shell
- **raw**: выполняет команду "как есть" без обработки, не требует Python на хосте
- **command**: выполняет команду без оболочки (/bin/sh), нет поддержки shell-операций
- **shell**: выполняет через оболочку, поддерживает пайпы/перенаправления

### 2. Добавление пользователей с SSH-ключами
1. Использовать модуль `authorized_key`:
   ```yaml
   - authorized_key:
       user: "{{ user }}"
       key: "{{ lookup('file', 'key.pub') }}"
   ```
2. Через shell:
   ```yaml
   - shell: echo "{{ key }}" >> /home/{{ user }}/.ssh/authorized_keys
   ```

### 3. Ограничение создания пользователей
- Группировка серверов в инвентаре:
  ```ini
  [user_servers]
  server1
  ```
- Условие в плейбуке:
  ```yaml
  when: inventory_hostname in groups['user_servers']
  ```

### 4. Установка Python без Python
```yaml
- raw: |
    if [ -f /etc/redhat-release ]; then
      yum install -y python3
    elif [ -f /etc/debian_version ]; then
      apt update && apt install -y python3
    fi
```

### 5. Структура роли
```
role/
├── defaults/  # переменные по умолчанию
├── vars/      # переопределяющие переменные
├── tasks/     # основные задачи
├── templates/ # jinja2 шаблоны
└── files/     # статические файлы
```

### 6. Разница vars/defaults
- `defaults/`: низкий приоритет (можно переопределить)
- `vars/`: высокий приоритет

### 7. Разница files/templates
- `files/`: статичные файлы (копируются как есть)
- `templates/`: jinja2 шаблоны (рендерятся с переменными)

### 8. Последовательное выполнение по хостам
```yaml
- hosts: all
  serial: 1  # по одному хосту
  tasks:
    - ...
```