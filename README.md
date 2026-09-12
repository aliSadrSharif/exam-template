# Scenario 1

Use one heading for each problem.
Write what was wrong and how you fixed it.
Paste the config you changed (only the changed part).
Paste the commands you used.
Write every step you tried, even guesses.

English is better. Persian is OK.

## Problem 1: (DNS resolve)

What was wrong:
DNS has been set to 127.0.0.1 and systemd.resolved was disable and stopped
also /etc/resolved.conf and /run/systemd/resolve/stub-resolve.conf was immutable

How I fixed it:

Config I changed

```
nameserver 127.0.0.53
```

Commands I used:

```
ping google.com #failed
cat /etc/resolved.conf #127.0.0.1
systemctl status systemd.resolved #disable and stopped
systemctl start systemd.resolved
systemctl enable systemd.resolved
vim /run/systemd/resolve/stub-resolve.conf #immutable
#making files mutable
chattr -i /etc/resolved.conf
chattr -i /run/systemd/resolve/stub-resolve.conf
rm /etc/resolved.conf /run/systemd/resolve/stub-resolve.conf
systemctl restart systemd.resolved #generates files again
ping google.com #successful
```

## Problem 2: (config nginx and docker compose file)

What was wrong:
nginx config wasnt connected to backend (port 8080 was wrong it should have been 5000)
nginx config api-backend:8080 should be changed to backend:5000
docker compose wasn't installed
docker compose backend wasn't connected to db network

How I fixed it:
i moved nginx.conf to /etc/nginx/nginx.conf #wasn't neccesary
nginx config: 
```
api-backend:8080 to backend:5000
```
docker compose: 
```
networks:
    - nginx-backend-net
    - backend-db-net
```

Commands I used:

```
apt update
apt install docker-compose-v2
vim nginx.conf #edit config
vim docker-compose.yml #edit docker compose
docker compose up -d
docker compose ps #make sure everything works
curl http://localhost #successful output
```




# Extra problems
i challenged myself to automate deploying an app using ansible this is my repo check it out:
https://github.com/aliSadrSharif/ansibleLab
it setups server, deploy frontend, backend and database with docker compose, adds ssl/tls self-signed and expose it via nginx service. more detailes are writen in README.md
i would be thankfull if you give a star for my repo
