Вот переписанный текст в формате, удобном для Obsidian, с сохранением структуры и добавлением возможностей для эффективного использования:

## CI / CD

### 1. Чем отличается Continuous Integration от Continuous Delivery от Continuous Deployment?
#cicd #definitions 

> [!summary] Ответ
> **Continuous Integration (CI)**  
> Практика автоматической интеграции изменений кода из ветки разработки в основную ветку
> 
> **Continuous Delivery (CD)**  
> Практика поддержки кода в состоянии, готовом к развертыванию на production
> 
> **Continuous Deployment**  
> Практика автоматического развертывания каждого изменения на production
> 
> ![[cicdcd.jpg]]
> 
> **Ключевое отличие**:  
> - При Continuous Deployment каждый шаг pipeline выполняется **автоматически**  
> - При Continuous Delivery перед деплоем на production требуется **ручное подтверждение**  
> 
> Пример pipeline:  
> 1. `Source Control` → 2. `Build` → 3. `Staging` → 4. `Production`  
> 
> Реализация ручного подтверждения:  
> - Кнопка в pipeline  
> - Бот в мессенджере  
> - Система аппрувов

---

### 2. Что означает конструкция `when: always` в stage блоке в GitLab CI?
#gitlab #ci-config 

> [!summary] Ответ
> Stage запускается **вне зависимости** от статуса предыдущих шагов.  
> Используется для:  
> - Нотификаций  
> - Очистки ресурсов  
> - Финализационных задач

---

### 3. Что выполняет конструкция `extends: .plan` в GitLab CI?
#gitlab #templates 

> [!summary] Ответ
> Механизм **повторного использования** конфигурации:  
> - `.plan` - имя шаблонной секции  
> - Выполняет скрипты из шаблона **первыми**  
> - Аналог функций в программировании

---

### 4. Как в GitLab CI сделать job только для ручного запуска?
#gitlab #manual-jobs 

> [!summary] Ответ
> Требуемая конфигурация:  
> ```yaml
> job_name:
>   when: manual
>   allow_failure: false
> ```
> 
> **Объяснение**:  
> - `when: manual` - активация только вручную  
> - `allow_failure: false` - блокировка pipeline до выполнения
```

### Особенности формата для Obsidian:
1. **Сворачиваемые блоки**  
Использование `> [!summary]` создает интерактивные сворачиваемые секции (требует включенного плагина Callouts)

2. **Внутренние ссылки**  
Изображение добавлено через `![[cicdcd.jpg]]` - будет автоматически искаться в папке заметки

3. **Теги**  
Добавлены хештеги (#cicd, #gitlab) для организации связей между заметками

4. **Подсветка кода**  
Конфигурационные блоки выделены через ```yaml``` для правильной подсветки синтаксиса

5. **Разделители**  
Секции разделены `---` для визуального структурирования

6. **Акценты**  
Ключевые термины выделены **жирным** для быстрого сканирования

Для работы изображений в Obsidian поместите файл cicdcd.jpg в ту же папку, где хранится заметка.