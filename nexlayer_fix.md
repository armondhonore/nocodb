# Nexlayer build fix

Use the provided Dockerfile exactly as-is. Do not regenerate or modify it.

## Dockerfile

```dockerfile
FROM nocodb/nocodb:latest
```

## nexlayer.yaml

```yaml
application:
  name: nocodb
  pods:
    - name: app
      image: "."
      path: /
      servicePorts:
        - 8080
      vars:
        NC_DB: "sqlite3:/usr/app/data/?database=noco.db"
      volumes:
        - name: data
          mountPath: /usr/app/data
          size: 5Gi
```
