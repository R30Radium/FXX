
В Kubernetes **пробы (probes)** используются для проверки состояния приложения в контейнере. Вот краткое объяснение каждого типа:

---

### 1. **Liveness Probe (Проба жизнеспособности)**
   - **Задача:** Проверить, работает ли приложение.
   - **Действие:** Если проба fails, Kubernetes перезапускает контейнер.
   - **Пример использования:** Приложение зависло и не отвечает.
   - **Пример настройки:**
     ```yaml
     livenessProbe:
       httpGet:
         path: /healthz
         port: 8080
       initialDelaySeconds: 3
       periodSeconds: 10
     ```

---

### 2. **Readiness Probe (Проба готовности)**
   - **Задача:** Проверить, готово ли приложение принимать трафик.
   - **Действие:** Если проба fails, Kubernetes временно убирает под из балансировки нагрузки.
   - **Пример использования:** Приложение запущено, но еще не готово обрабатывать запросы.
   - **Пример настройки:**
     ```yaml
     readinessProbe:
       httpGet:
         path: /ready
         port: 8080
       initialDelaySeconds: 5
       periodSeconds: 10
     ```

---

### 3. **Startup Probe (Проба старта)**
   - **Задача:** Проверить, завершился ли запуск приложения.
   - **Действие:** Если проба fails, Kubernetes ждет, пока приложение не станет готово, прежде чем запускать liveness и readiness пробы.
   - **Пример использования:** Приложение с долгим временем запуска.
   - **Пример настройки:**
     ```yaml
     startupProbe:
       httpGet:
         path: /startup
         port: 8080
       failureThreshold: 30
       periodSeconds: 10
     ```

---

### **Основные отличия:**
| **Тип пробы**   | **Когда используется**                          | **Что происходит при ошибке**                  |
|------------------|------------------------------------------------|-----------------------------------------------|
| **Liveness**     | Приложение зависло или не работает.            | Контейнер перезапускается.                    |
| **Readiness**    | Приложение не готово принимать трафик.         | Под временно исключается из балансировки.     |
| **Startup**      | Приложение долго запускается.                  | Kubernetes ждет, пока приложение не запустится. |

---

### **Когда использовать:**
- **Liveness:** Для перезапуска "мертвых" контейнеров.
- **Readiness:** Для управления трафиком, если приложение временно не готово.
- **Startup:** Для приложений с долгим временем запуска, чтобы избежать ложных срабатываний liveness и readiness проб.

Эти пробы помогают Kubernetes эффективно управлять жизненным циклом приложений.