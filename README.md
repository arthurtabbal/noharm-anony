# Serviço Local de Remoção de Nomes

Serviço que anonimiza nomes pessoais oriundos de evoluções dos diversos profissionais (médicos, enfermeiros, farmacêuticas, etc.), substituindo os nomes por \*\*\*.

### 1. Run Docker

```
git clone https://github.com/noharm-ai/noharm-anony

cd noharm-anony

docker build -t anony . #build

docker run -p 80:80 anony #test

docker run -d --log-opt max-size=100m --name myanony -p 80:80 anony #deamon
```

### 1.1. Identificar o IP e alterar no Remote URL do Nifi (no InvokeHTTP)

```
docker network inspect bridge
```

### 1.2. Testar Plain

```
curl -X PUT -H 'Accept: application/json' -H 'Content-Type: application/json' http://localhost/clean -d '{"text" : "FISIOTERAPIA TRAUMATO - MANHÃ  Henrique Dias, 38 anos. Exercícios metabólicos de extremidades inferiores. Realizo mobilização patelar e leve mobilização de flexão de joelho conforme liberado pelo Dr Marcelo Arocha. Oriento cuidados e posicionamentos."}'
```

### 1.2. Testar RTF

```
curl -X PUT -H 'Accept: application/json' -H 'Content-Type: application/json' http://localhost/clean -d '{"text" : "{\\rtf1\\ansi\\b FISIOTERAPIA TRAUMATO - MANHÃ  Henrique Dias, 38 anos.\\b0.\\par \\i Exercícios metabólicos de extremidades inferiores. Realizo mobilização patelar e leve mobilização de flexão de joelho conforme liberado pelo Dr Marcelo Arocha. Oriento cuidados e posicionamentos.\\i0.}"}'
```

### 2. Outras configurações

### 2.1. Run Network Docker

```
docker network create --subnet=172.19.0.0/16 noharm-net

docker network connect noharm-net nifi

docker run -d --log-opt max-size=100m --name myanony --net noharm-net --ip 172.19.0.3 -p 80:80 anony
```

### 2.2. Run Limited Memory Docker

```
docker run -d --name myanony -m 2g --memory-swap="2g" -p 80:80 anony
```

### 2.3 RTF Format

RTF should be detected automatically but you can force the input to be handled as a RTF using the FORMAT parameter. Example:

```
curl -X PUT -H 'Accept: application/json' -H 'Content-Type: application/json' http://localhost/clean -d '{"format": "rtf", "text" : "FISIOTERAPIA TRAUMATO - MANHÃ  Henrique Dias, 38 anos. Exercícios metabólicos de extremidades inferiores. Realizo mobilização patelar e leve mobilização de flexão de joelho conforme liberado pelo Dr Marcelo Arocha. Oriento cuidados e posicionamentos."}'
```

### 2.4 Development

```
$ python3 -m venv env
$ source env/bin/activate
$ pip3 install -r requirements.txt
```
