FROM hypriot/rpi-alpine-scratch

RUN apk update && apk upgrade && apk add bash && rm -rf /var/cache/apk/* && apk add --update python3 

RUN pip3 install pelican markdown

CMD [/bin/bash]
