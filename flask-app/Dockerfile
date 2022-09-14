FROM alpine:3.15 AS first
COPY app.py ./flask-app/app.py
COPY package.json ./flask-app/package.json
COPY package-lock.json ./flask-app/package-lock.json
COPY requirements.txt ./flask-app/requirements.txt
COPY webpack.config.js ./flask-app/webpack.config.js

#instaluje pakiety
#RUN apk add npm  

#RUN npm install -g webpack webpack-cli

RUN apk add npm
#uruchamia npm
WORKDIR /flask-app
RUN npm install
RUN npm run build 



FROM alpine:3.15
COPY --from=first /flask-app ./flask-app
ENV PYTHONUNBUFFERED=1
RUN apk add --no-cache python2 && \
    python -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip install --upgrade pip setuptools && \
    rm -r /root/.cache

RUN pip install -r flask-app/requirements.txt
ENTRYPOINT ["python2","flask-app/app.py"]
