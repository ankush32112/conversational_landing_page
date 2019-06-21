# Makerobos landing page

## Create free account in makerobos.com

visit https://www.makerobos.com/user/register to create a new account or jusg login if you already have one

## Getting bot-id
after login you would be redirected to makerobos dashboard
1. just pick any plan, then pick a prebuilt bot from the templates or pick th blank one if you wish to create from scratch.
2. Enter bot details like bot name, and website name where you want to run you bot, if you don't have a website just enter any valid web address (you can change it later anytime)
4. just click Next.. Next.. 
5. Your bot will be ready and displayed as a card in your dashboard
6. click on the card. in the url you can see the 16 char alphanumeric bot-id. ex: DS6SDF7D78DFDFG6


If you want to run the Makerobos conversational landing page in you your server, just pull this docker image and pass the bot-id as environment variable with key BOT

## Example for docker:
```
docker run -e BOT=<bot-id> -p 127.0.0.1:8000:4800/tcp makerobos/landing_page:latest node dist/server
```

## Example of docker-compose:
```yml
version: '3'
services:
    landing_page:
        image: makerobos/landing_page:latest
        command: "node dist/server"
        ports:
             - "8000":"4800"
        environment:
             - BOT=<bot-id>
```


then you can proxy pass to you web server
here is how you can do it using nginx:

## Nginx server conf
```
server {
    server_name your.server.com;
    location / {
        proxy_pass http://127.0.0.1:8000$request_uri;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        add_header locationischat ank;
        proxy_redirect     off;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Host $server_name;
    }
}
```
