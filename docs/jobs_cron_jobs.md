## [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

A Job runs a task to completion — once or multiple times, depending on settings.

They temporarily run as pods on nodes to handle specific tasks.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

## [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

A CronJob runs Jobs on a schedule, like a Linux cron job. Each run creates a Pod that executes the task.

They temporarily run as pods on nodes to handle specific tasks.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```