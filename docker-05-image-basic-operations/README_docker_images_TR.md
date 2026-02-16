# Hands-on Docker-05 : Docker Image Temel İşlemleri

Bu uygulamalı eğitimin amacı, öğrencilere Docker'daki image'lar hakkında bilgi vermektir.

## Öğrenim Hedefleri

Bu uygulamalı eğitimin sonunda öğrenciler;

- Docker image'ının ne olduğunu açıklayabilecek.

- Docker image katmanlarını açıklayabilecek.

- Docker'daki image'ları listeleyebilecek.

- Docker Hub'ı açıklayabilecek.

- Docker Hub hesabına sahip olup repository oluşturabilecek.

- Docker image'larını çekebilecek (pull).

- Image tag'lerini açıklayabilecek.

- Docker image'ını inceleyebilecek (inspect).

- Docker image'larını arayabilecek.

- Dockerfile'ın ne olduğunu açıklayabilecek.

- Dockerfile ile image oluşturabilecek (build).

- Image'ları Docker Hub'a yükleyebilecek (push).

- Docker image'larını silebilecek.

## İçindekiler

- Bölüm 1 - Docker Machine Instance Başlatma ve SSH ile Bağlanma

- Bölüm 2 - Docker Image Komutlarını ve Docker Hub'ı Kullanma

- Bölüm 3 - Dockerfile ile Docker Image'ları Oluşturma

## Bölüm 1 - Docker Machine Instance Başlatma ve SSH ile Bağlanma

- SSH bağlantılarına izin veren security group ile Amazon Linux 2 AMI üzerinde bir Docker makinesi başlatın. [Cloudformation Template for Docker Machine Installation](../docker-01-installing-on-ec2-linux2/docker-installation-template.yml) kullanabilirsiniz.

- SSH ile instance'ınıza bağlanın.

```bash
ssh -i .ssh/mykey.pem ec2-user@ec2-3-133-106-98.us-east-2.compute.amazonaws.com
```

## Bölüm 2 - Docker Image Komutlarını ve Docker Hub'ı Kullanma

- Docker Hub'ın ne olduğunu açıklayın.

- Docker Hub'a kaydolun ve kullanıcı arayüzünü açıklayın.

- `flask-app` adında ve `This image repo holds Flask apps.` açıklamasıyla bir repository oluşturun.

- EC2 instance'ında docker servisinin çalışır durumda olup olmadığını kontrol edin.

```bash
systemctl status docker
```

- Docker'daki image'ları listeleyin ve image özelliklerini açıklayın.

```bash
docker image ls
```

- Docker image `ubuntu`'yu indirin ve Docker Hub'daki image tag'lerini açıklayın (varsayılan olarak `latest`). Docker Hub'da `ubuntu` repo'sunu gösterin ve `latest` tag'inin hangi versiyona karşılık geldiğini gösterin (`22.04`).

```bash
# Defaults to ubuntu:latest
docker image pull ubuntu    
docker image ls
```

- `ubuntu`'yu interactive shell açık olarak container olarak çalıştırın.

```bash
docker run -it ubuntu
```

- Container'da `ubuntu` işletim sistemi bilgisini gösterin (`VERSION="22.04.2 LTS (Jammy Jellyfish)"`) ve `latest` tag'inin Docker Hub'daki gibi `22.04` sürümünü gösterdiğini not edin. Ardından container'dan çıkın.

```bash
cat /etc/os-release
exit
```

- Docker Hub'da `22.04` olarak etiketlenmiş `ubuntu` image'ının önceki versiyonunu (`22.04`) indirin ve image listesini açıklayın.

```bash
docker image pull ubuntu:22.04
docker image ls
```

- `ubuntu` image'ını inceleyin (inspect) ve özelliklerini açıklayın.

```bash
# Defaults to ubuntu:latest
docker image inspect ubuntu
# Ubuntu with tag 22.04
docker image inspect ubuntu:22.04
```

- Hem `bash` üzerinde hem de Docker Hub'da Docker Image'larını arayın.

```bash
docker search ubuntu
```

## Bölüm 3 - Dockerfile ile Docker Image'ları Oluşturma

- `python:alpine` image'ı temel alarak Dockerfile ile bir Python Flask uygulaması image'ı oluşturun ve Docker Hub'a yükleyin.

- Docker image oluşturmak için gerekli tüm dosyaları tutacak bir klasör oluşturun.

```bash
mkdir my_web
cd my_web
```

- Uygulama kodunu oluşturun ve dosyaya kaydedin, adını `welcome.py` koyun.

```bash
echo '
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "<h1>Welcome to Docker Lesson</h1>"
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)
' > welcome.py
```

- Gerekli paketleri ve modülleri listeleyen bir Dockerfile oluşturun ve adını `Dockerfile` koyun.

```Dockerfile
FROM ubuntu
RUN apt-get update -y
RUN apt-get install python3 -y
RUN apt-get install python3-pip -y
RUN pip3 install Flask --break-system-packages
COPY . /app
WORKDIR /app
CMD python3 ./welcome.py
```

- Dockerfile'dan yerel olarak Docker image oluşturun, `<Your_Docker_Hub_Account_Name>/flask-app:1.0` olarak etiketleyin ve oluşturma adımlarını açıklayın. Repo adının `<Your_Docker_Hub_Account_Name>/<Your_Image_Name>` kombinasyonu olduğunu unutmayın.

```bash
docker build -t babaktanriverdi/flask-app:1.0 .
docker image ls
```

- Yeni oluşturulan image'ı detached modda container olarak çalıştırın, host `port 80`'i container `port 80`'e bağlayın ve container'ı `welcome` olarak adlandırın. Ardından çalışan container'ları listeleyin ve tarayıcıdan EC2 instance'ına bağlanarak Flask uygulamasının çalıştığını gösterin.

```bash
docker run -d --name welcome -p 80:80 babaktanriverdi/flask-app:1.0
docker ps
```

- Docker'a kimlik bilgilerinizle giriş yapın.

```bash
docker login
```

- Yeni oluşturulan image'ı Docker Hub'a yükleyin (push) ve Docker Hub'da güncellenmiş repo'yu gösterin.

```bash
docker push babaktanriverdi/flask-app:1.0
```

- Bu sefer image'ın boyutunu küçülteceğiz.

- Gerekli paketleri ve modülleri listeleyen bir Dockerfile oluşturun ve adını `Dockerfile-alpine` koyun.

```Dockerfile
FROM python:alpine
RUN pip install flask
COPY . /app
WORKDIR /app
EXPOSE 80
CMD python ./welcome.py
```

- Dockerfile'dan yerel olarak Docker image oluşturun, `<Your_Docker_Hub_Account_Name>/<Your_Image_Name>:<Tag>` olarak etiketleyin ve oluşturma adımlarını açıklayın. Repo adının `<Your_Docker_Hub_Account_Name>/<Your_Image_Name>` kombinasyonu olduğunu unutmayın.

```bash
docker build -t babaktanriverdi/flask-app:2.0 -f ./Dockerfile-alpine .
docker image ls
```

- `babaktanriverdi/flask-app:1.0`'ın boyutu yaklaşık 473MB iken, `babaktanriverdi/flask-app:2.0`'ın boyutunun 67MB olduğunu not edin.

- Yeni oluşturulan image'ı detached modda container olarak çalıştırın, host'un `port 8080`'ini container'ın `port 80`'ine bağlayın ve container'ı `welcome2` olarak adlandırın. Ardından çalışan container'ları listeleyin ve tarayıcıdan EC2 instance'ına bağlanarak Flask uygulamasının çalıştığını gösterin.

```bash
docker run -d --name welcome2 -p 8080:80 babaktanriverdi/flask-app:2.0
docker ps
```

- `welcome2` container'ını durdurun ve kaldırın.

```bash
docker stop welcome2 && docker rm welcome2
```

- Yeni oluşturulan image'ı Docker Hub'a yükleyin (push) ve Docker Hub'da güncellenmiş repo'yu gösterin.

```bash
docker push babaktanriverdi/flask-app:2.0
```

- Aynı image'ı farklı tag'lerle de etiketleyebiliriz.

```bash
docker image tag babaktanriverdi/flask-app:2.0 babaktanriverdi/flask-app:latest
```

- Image'ı `image id` ile yerel olarak silin.

```bash
docker image rm 497
```


```bash
# 1. Docker Hub'a giriş yap
docker login

# 2. Dockerfile-alpine kullanarak image'ı build et
docker build -t babaktanriverdi/flask-app:2.0 -f ./Dockerfile-alpine .

# 3. Image'ı Docker Hub'a push et
docker push babaktanriverdi/flask-app:2.0

# 4. (İsteğe bağlı) Image'ın oluştuğunu kontrol et
docker images | grep flask-app
```



```bash
# 1. Docker Hub'a giriş yap
docker login

# 2. Image'ı build et
docker build -t flask-app:1.0 .

# 3. Image'ı Docker Hub kullanıcı adınla tag'le
docker tag flask-app:1.0 babaktanriverdi/flask-app:1.0

# 4. Image'ı Docker Hub'a push et
docker push babaktanriverdi/flask-app:1.0
```