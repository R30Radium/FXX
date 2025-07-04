### Kubernetes: Вопросы и Ответы 

---

#### 1. Чем отличается Kubernetes от Openshift?
- **Openshift** имеет более строгие политики безопасности и модели аутентификации.
- **Openshift** поддерживает полную интеграцию CI/CD Jenkins.
- **Openshift** имеет веб-консоль по умолчанию. В Kubernetes консоль необходимо дополнительно устанавливать.
- В **Kubernetes** возможно устанавливать сторонние сетевые плагины. В Openshift используется собственное сетевое решение Open vSwitch (3 плагина).
- **Kubernetes** может быть установлен практически на любой дистрибутив Linux. Openshift преимущественно использует RH-дистрибутивы.
- **Kubernetes** доступен в большинстве облачных платформ (GCP, AWS, Azure, Yandex.Cloud). Openshift доступен на Azure и IBM Cloud.
- В **Openshift** поды по умолчанию запускаются под обычным пользователем. Для запуска под root нужны права сервисного аккаунта. В Kubernetes поды могут запускаться под root.  
[Подробнее](https://www.redhat.com/cms/managed-files/cl-openshift-and-kubernetes-ebook-f25170wg-202010-en.pdf)

---

#### 2. Чем отличаются *ReplicationController* от *ReplicaSet*?
- **ReplicationController** гарантирует, что указанное количество реплик подов будет работать одновременно.
- **ReplicaSet** — следующее поколение ReplicationController. Ключевое отличие: ReplicaSet поддерживает множественный выбор в селекторе (set-based selectors), тогда как ReplicationController использует только выбор на основе равенства (equality-based selectors).

---

#### 3. Если на каждой ноде Kubernetes кластера нужно запустить контейнер, то какой ресурс Kubernetes вам подойдет?
- **DaemonSet** — контроллер, который гарантирует запуск пода на всех (или выбранных) нодах кластера. Автоматически добавляет/удаляет поды при изменении количества нод.  
  *Примеры использования:* сбор логов (Fluentd), мониторинг (Node Exporter).

---

#### 4. Как поды разнести на разные ноды?
- Использовать **podAntiAffinity** в спецификации пода. Пример:
  ```yaml
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: "app"
                operator: In
                values: ["my-app"]
          topologyKey: "kubernetes.io/hostname"
  ```

---

#### 5. В облаке есть 3 зоны доступности. Как сделать так, чтобы поды приложения распределились по этим зонам равномерно?
- Использовать **topologySpreadConstraints** с указанием лейбла зоны (например, `topology.kubernetes.io/zone`):
  ```yaml
  topologySpreadConstraints:
    - maxSkew: 1
      topologyKey: topology.kubernetes.io/zone
      whenUnsatisfiable: ScheduleAnyway
      labelSelector:
        matchLabels:
          app: my-app
  ```
- Альтернатива: **podAntiAffinity** с `topologyKey: topology.kubernetes.io/zone`.

---

#### 6. Как контейнеры одного пода разнести на разные ноды?
- **Невозможно**. Под — неделимая сущность Kubernetes. Все контейнеры в поде всегда запускаются на одной ноде.

---

#### 7. Как обеспечить, чтобы поды никогда не перешли в состояние Evicted на ноде?
- Настроить **QoS класса Guaranteed**:
  - Указать равные значения `limits` и `requests` для CPU/memory.
  - Пример:
    ```yaml
    resources:
      limits:
        cpu: "1"
        memory: "1Gi"
      requests:
        cpu: "1"
        memory: "1Gi"
    ```
- Использовать **PodDisruptionBudget (PDB)** для защиты от добровольных вытеснений:
  ```yaml
  apiVersion: policy/v1
  kind: PodDisruptionBudget
  metadata:
    name: my-pdb
  spec:
    minAvailable: 1
    selector:
      matchLabels:
        app: my-app
  ```
  [Документация](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/#create-a-pod-that-gets-assigned-a-qos-class-of-guaranteed)

---

#### 8. За что отвечает kube-proxy?
- Обеспечивает сетевое взаимодействие между сервисами и подами:
  - Реализует балансировку трафика через iptables/IPVS.
  - Отслеживает изменения Endpoints и обновляет правила.

---

#### 9. Что находится на master-ноде?
- **kube-apiserver**: Принимает API-запросы.
- **etcd**: Распределённое хранилище состояния кластера.
- **kube-scheduler**: Распределяет поды по нодам.
- **kube-controller-manager**: Запускает контроллеры (Node, Replication и др.).
- *Примечание:* На master-нодах по умолчанию не размещаются пользовательские поды (можно изменить через taints).

---

#### 10. Что находится на worker-ноде?
- **kubelet**: Управляет жизненным циклом контейнеров.
- **kube-proxy**: Обеспечивает сетевую коммуникацию.
- **Container Runtime**: Docker, containerd, или CRI-O.
- Пользовательские поды.

---

#### 11. Как установить Kubernetes?
- Через **kubeadm** ([инструкция](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)).
- С помощью **Kubespray** (Ansible-решение: [github](https://github.com/kubernetes-sigs/kubespray)).

---

#### 12. Чем отличается *StatefulSet* от *Deployment*?
- **Deployment**: Для stateless-приложений. Все реплики используют общий том (если PVC подключен).
- **StatefulSet**: Для stateful-приложений (БД, Kafka). Каждая реплика имеет уникальный:
  - Сетевой идентификатор (pod-0, pod-1).
  - Персистентный том (PVC создаётся через `volumeClaimTemplates`).

---

#### 13. Что такое *операторы* в понятиях Kubernetes?
- **Оператор** — расширение Kubernetes, автоматизирующее управление сложными stateful-приложениями (например, БД).
  - Работает через Custom Resource Definitions (CRD).
  - Реагирует на события в API Kubernetes.
  *Примеры:* Prometheus Operator, Elasticsearch Operator.

---

#### 14. Почему *DaemonSet* не нужен scheduler?
- **DaemonSet** сам управляет размещением подов:
  - Гарантирует запуск пода на всех нодах (или подмножестве через `nodeSelector`).
  - Контролирует добавление/удаление нод.
  *Примечание:* Scheduler участвует в процессе, но логика размещения предопределена.

---

#### 15. В каких случаях не отработает перенос пода на другую ноду?
- Если на ноде:
  - Недостаточно ресурсов (CPU, memory).
  - Несоответствие taints/tolerations.
  - Невыполнение правил affinity/anti-affinity.
  - Нода в состоянии `NotReady`.

---

#### 16. Что делает *ControllerManager*?
- Запускает контроллеры, которые следят за состоянием кластера:
  - **Node Controller**: Мониторит статус нод.
  - **Replication Controller**: Поддерживает количество реплик подов.
  - **Endpoint Controller**: Связывает сервисы и поды.
  - Другие (Namespace, ServiceAccount).

---

#### 17. Администратор выполняет команду `kubectl apply -f deployment.yaml`. Опишите по порядку что происходит?
1. **kubectl** отправляет манифест в **kube-apiserver**.
2. **kube-apiserver** сохраняет состояние в **etcd**.
3. **kube-controller-manager** создаёт **ReplicaSet**.
4. **kube-scheduler** выбирает ноду для пода.
5. **kubelet** на worker-ноде запускает контейнеры через container runtime.
6. **kube-proxy** обновляет сетевые правила.

---

#### 18. Как выполнить обновление Kubernetes в контуре без интернета?
1. На машине с интернетом:
   - Скачать пакеты: `yumdownloader --resolve kubeadm kubelet kubectl`.
   - Сохранить образы: `docker save <image> > image.tar`.
2. В offline-контуре:
   - Установить пакеты: `yum install ./*.rpm`.
   - Загрузить образы: `docker load < image.tar`.
   - Инициализировать кластер: `kubeadm init --apiserver-advertise-address=<IP>`.
   - Установить CNI (например, Weave).

---

#### 19. Чем Router в Openshift отличается от Ingress в Kubernetes?
- **Router** в Openshift: Реализация на базе HAProxy.
- **Ingress** в Kubernetes: Абстракция, поддерживающая разные реализации (Nginx, Traefik, HAProxy).
  *Фактически:* В Openshift Router — это частный случай Ingress-контроллера.

---

#### 20. Почему для установки Kubernetes требуется отключить swap?
- Планировщик Kubernetes не может корректно оценить доступные ресурсы при включённой swap-памяти. Это приводит к нестабильности и проблемам с производительностью.

---

#### 21. Что такое *Pod* в Kubernetes?
- **Pod** — минимальная deployable-единица:
  - Группа из одного или нескольких контейнеров.
  - Разделяет сетевое пространство (IP, порты) и хранилище (volumes).
  - Контейнеры в поде всегда запускаются на одной ноде.

---

#### 22. Сколько контейнеров запускается в одном поде?
- **N+1**, где N — пользовательские контейнеры, +1 — служебный контейнер `pause` (создаёт общее сетевое пространство).

---

#### 23. Для чего нужен *pause* контейнер в каждом поде?
- Создаёт общее сетевое пространство имён (netns) для всех контейнеров пода.
- Удерживает сетевые настройки, пока жив хотя бы один контейнер.

---

#### 24. Чем отличается *Deployment* от *DeploymentConfig* (Openshift)?
- **Deployment** (Kubernetes): Использует ReplicaSet для управления репликами.
- **DeploymentConfig** (Openshift): 
  - Поддерживает триггеры (например, на изменение ImageStream).
  - Имеет собственные стратегии развёртывания (Custom, Recreate, Rolling).
  - Хранит историю развёртываний в etcd.  
[Документация](https://docs.openshift.com/container-platform/4.1/applications/deployments/what-deployments-are.html)

---

#### 25. Для чего нужны *Startup*, *Readiness*, *Liveness* пробы? Чем отличаются?
- **Liveness**: Проверяет, что контейнер работает. При падении — контейнер перезапускается.
- **Readiness**: Проверяет, готов ли контейнер принимать трафик. При падении — под исключается из балансировки сервиса.
- **Startup**: Проверяет запуск "тяжёлых" контейнеров. Блокирует liveness/readiness до успешного старта.

---

#### 26. Чем отличаются *Taints* и *Tolerations* от *Node Afiinity*?
- **Node Affinity**: "Притягивает" поды к нодам с определёнными метками (`nodeSelector`).
- **Taints и Tolerations**:
  - **Taint** (на ноде): "Отталкивает" все поды, кроме тех, у которых есть matching **Toleration**.
  - **Toleration** (у пода): Разрешает запуск на "загрязнённой" ноде, но не гарантирует размещение.
  *Ключевое:* Affinity — позитивное правило, Taints — негативное.

---

#### 27. Чем отличаются *Statefulset* и *Deployment* в плане стратегии обновления подов Rolling Update?
- **Deployment**:
  - Создаёт новый под -> переключает трафик -> удаляет старый под.
- **StatefulSet**:
  - По умолчанию: Обновляет поды в обратном порядке (начиная с последней реплики).
  - Поддерживает стратегию `Partition` для канареечных развёртываний.

---

#### 28. Для чего в Kubernetes используются порты 2379 и 2380?
- **2379/tcp**: Клиентские подключения к etcd (для kube-apiserver).
- **2380/tcp**: Внутренняя коммуникация между нодами etcd в кластере.

---

#### 29. Как разместить контейнеры `nginx` и `redis` пода на разных нодах?
- **Невозможно**. Контейнеры в рамках одного пода всегда запускаются на одной ноде.

---

#### 30. Какую функцию выполняют `indent` и `nindent` в Helm чартах?
- **`indent`**: Добавляет отступ к каждой строке текста:
  ```yaml
  {{ .Values.text | indent 4 }}  # Добавит 4 пробела в начале каждой строки.
  ```
- **`nindent`**: Делает то же, что `indent`, но добавляет перенос строки в начало:
  ```yaml
  {{ .Values.text | nindent 4 }}  # -> "\n    текст".
  ```

---

#### 31. Чем отличается *Deployment* от *ReplicaSet*?
- **ReplicaSet**: Гарантирует количество реплик подов.
- **Deployment**: Надстройка над ReplicaSet, добавляющая:
  - Декларативные обновления (rolling update, rollback).
  - Историю изменений (через `kubectl rollout history`).

---

#### 32. Чем отличается *Deployment* от *StatefulSet*?
- **Deployment**:
  - Для stateless.
  - Подключает общий том (если PVC с `ReadWriteMany`).
  - Случайные имена подов (e.g., `app-5fdd978f6d`).
- **StatefulSet**:
  - Для stateful.
  - Создаёт уникальный PVC для каждого пода (`volumeClaimTemplates`).
  - Детерминированные имена подов (e.g., `db-0`, `db-1`).

---

#### 33. Что такое HPA (Horizontal Pod Autoscaling)? Как он работает и что для этого нужно?
- **HPA**: Автоматически масштабирует количество подов на основе метрик (CPU, memory, custom).
- **Требования**:
  - Установленный `metrics-server` (для CPU/memory).
  - Для кастомных метрик — адаптеры (e.g., Prometheus Adapter).
- **Принцип работы**:
  1. HPA запрашивает метрики у metrics-server.
  2. Сравнивает с целевым значением (targetAverageUtilization).
  3. Изменяет количество реплик в Deployment/StatefulSet.

---

#### 34. Что такое Headless сервис?
- Сервис с `clusterIP: None`.
- Не имеет виртуального IP. DNS-запрос возвращает IP всех подов сервиса.
- **Использование**: Для прямого доступа к подам (e.g., StatefulSet, кластеры БД).

---

#### 35. Что такое ExternalName сервис?
- Сервис типа `ExternalName`:
  ```yaml
  kind: Service
  spec:
    type: ExternalName
    externalName: example.com
  ```
- Создаёт CNAME-запись в DNS (e.g., `my-svc.namespace.svc.cluster.local → example.com`).

---

#### 36. Что такое ExternalIP сервис?
- Позволяет назначить сервису внешний IP:
  ```yaml
  spec:
    externalIPs:
      - 203.0.113.42
  ```
- Трафик на `203.0.113.42:порт` перенаправляется на сервис.

---

#### 37. Что такое NodePort сервис?
- Открывает порт (30000-32767) на всех нодах кластера.
- **Пример**:
  ```yaml
  spec:
    type: NodePort
    ports:
      - port: 80
        nodePort: 30080
  ```
- Доступ: `<node-ip>:30080`.

---

#### 38. Что такое capabilities? Для чего нужно их описывать?
- **Capabilities** — флаги прав ядра Linux (e.g., `NET_ADMIN`, `SYS_TIME`).
- **Зачем**:
  - Запускать контейнеры с минимальными привилегиями (без `privileged: true`).
  - Гранулярно управлять доступом к системным вызовам.
  **Пример**:
  ```yaml
  securityContext:
    capabilities:
      add: ["NET_ADMIN"]
      drop: ["ALL"]
  ```