# Jenkins CI/CD pipeline for Chef cookbooks


1. Initializa docker-compose

```
% docker compose up -d
```

2. Reconfigure chef-server

```
% docker exec -it chef-server chef-server-ctl reconfigure 
```

