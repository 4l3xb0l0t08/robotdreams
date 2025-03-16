## Домашне завдання №11

### Перелік файлів

1. HomeWork11.pdf - Опис всього завдання цілком та покроково у форматі pdf
2. HomeWork11.txt - Опис всього завдання цілком та покроково у форматі txt
3. bb-pv.yaml - файл для створення Persistent Volume для BusyBox
4. bb-pvс.yaml - файл для створення Persistent Volume Claim для BusyBox
5. bb-config.yaml - файл налаштувань для BusyBox
6. bb-deployment.yaml - файл деплойменту BusyBox
7. bb-svc.yaml - файл сервісу для BusyBox
8. bb-hpa.yaml - файл Horizontal Pod Autoscaler для BusyBox
9. ng-pv.yaml - файл для створення Persistent Volume для NGINX
10. ng-pvс.yaml - файл для створення Persistent Volume Claim для NGINX
11. nginx-configmap.yaml - файл налаштувань для NGINX
12. ng-deployment.yaml - файл деплойменту NGINX
13. ng-service.yaml - файл сервісу для NGINX
14. nginx-ingress.yaml - файл налаштування ingress для NGINX
15. loki-pv.yaml - файл для створення Persistent Volume для Loki
16. loki-pvс.yaml - файл для створення Persistent Volume Claim для Loki
17. loki-config.yaml - файл налаштувань для Loki
18. loki-deployment.yaml - файл деплойменту Loki
19. loki-service.yaml - файл сервісу для Loki
20. fluentbit-config.yaml - файл налаштувань для FluentBit
21. fluentbit-deployment.yaml - файл деплойменту (DaemonSet) для FluentBit
22. grafana-deployment.yaml - файл деплойменту Grafana
23. grafana-service.yaml - файл сервісу для Grafana
24. grafana-ingress.yaml - файл налаштування ingress для Grafana
25. prom-config.yaml - файл налаштувань для Prometheus
26. prom-deployment.yaml - файл деплойменту Prometheus
27. prom-service.yaml - файл сервісу для Prometheus
28. kube-state-metrics.yaml - файл сукупних налаштувань для деплойменту метрик
29. node-exporter.yaml - файл деплойменту (DaemonSet) для налаштування node-exporter у Prometheus
30. readme.md - Цей файл, шо містить опис вмісту теки

### Файл bb-deployment з коментарями для кожної директиви
Орігінальний файл не містить коментарів, шоб раптом не зрушити форматування

	apiVersion: apps/v1 #API-версія Kubernetes
	kind: Deployment #Тип ресурсу Deployment
	metadata: #Містить метаінформацію про Deployment
	  name: bb-deployment #Ім'я, має бути унікальним
	  namespace: bb-namespace #Унікальна назва простору імен у кластері. Використовують для логічного розділення ресурсів Kubernetes
	spec:  #Основний розділ конфігурації
	  replicas: 3 #Кількість контейнерів, які повинні запуститися
	  selector: #Визначає мітки, згідно яких до Pod застосовуються цей Deployment (або сервіс чи сервіси які будуть працювати у одному просторі імен)
	    matchLabels: #Перелік міток які мають мати Pod, для того, шоб до них був застосований цей Deployment
	      app: bb-app #Ця мітка має бути у Pod шоб до нього був застосований цей Deployment. Таку мітку буде мати й кожний под створений за допомогою цього деплойменту
	  template: #Шаблон нового Pod
	    metadata: #Мітки для ідентифікації Pod
	      labels: #Мітки які додаються до Pod
	        app: bb-app #Ця мітка буде у кожного новоствореного Pod
	    spec: #Конфігурація Pod, зокрема список контейнерів
	      volumes: #Перелік томів яки будуть запитані для монтування у Pod
	      - name: bb-pvc #Назва тома 
	        persistentVolumeClaim: #Перелік PVC які будуть запитані для монтування у Pod
	          claimName: bb-pvc #Ім'я PVC який буде запитан для створювання розділу у Pod
	      containers: #Список контейнерів
	      - name: busybox #Назва контейнера (часткова, по цій назві зокрема ми зможемо знайти наші контейнери серед решти)
	        image: busybox:1.28 #Імідж, який буде використано для створення контейнера
	        ports: #Список портів, що відкриваються в контейнері
	        - containerPort: 80 #Порт, який застосовує контейнер
	        resources: #Ресурси які виділяються для роботи кожного контейнера у поді
	          requests: #Мінімальний (початковий) ресурс шо має бути виділений контейнеру. Якшо цього ресурсу немає, контейнер не стартує і буде очікувати вивельнення ресурсу
	            cpu: "250m" #Ресур ЦП необхідний для роботи або старту контейнера
	            memory: "128Mi" #Ресурс оперативної пам'яті необхідний для роботи або старту контейнера
	          limits: #Максимальний ліміт ресурсів які можуть бути виделені контейнеру, наприклад у час пікових навантажень
	            cpu: "500m" #Максимальний ресурс ЦП
	            memory: "256Mi" #Максимальний об'єм оперативної пам'ті
	        envFrom: # Перелік всіх ConfigMap які містять змінні середовища контейнера 
	          - configMapRef: #Перелік посилань на ConfigMap
	              name: bb-config #Ім'я ConfigMap який містить змінні середовища для контейнера (контейнерів)
	        command: #Команда яку буде виконано коли под буде створено й запущено
	          - "/bin/sh"
	          - "-c"
	          - 'while true; do echo "$(hostname) : $(date)"; echo "$(hostname) : $(date)" >> "/mnt/data/busybox/$(MSGFILE)"; sleep 5; done'
	        volumeMounts: #Точки монтування томів
	        - mountPath: /mnt/data/busybox #Монтуемо цей каталог на хостовому комп'ютері до volume з цім bb-pvc ім'ям 
	          name: bb-pvc #Ім'я volume який монтується до вишче вказаного каталогу
