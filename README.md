# Laboratorio Ansible con Docker

## Objetivo

Administrar dos contenedores Ubuntu 24.04 mediante Ansible. El Playbook obtiene
información de cada servidor, crea un directorio y un archivo de información,
crea tres subdirectorios mediante un `loop` y muestra un mensaje condicionado
a que el sistema operativo sea Ubuntu.

## Requisitos

- Docker Engine o Docker Desktop con el motor Linux habilitado.
- Ansible instalado en el equipo controlador (recomendado: Ubuntu en WSL 2).
- Colección `community.docker`.
- SDK de Docker para Python en el controlador.
- Python 3 instalado en ambos contenedores.

## Preparación del controlador (Ubuntu / WSL)

Instalar Ansible y las dependencias:

```bash
sudo apt update
sudo apt install -y python3 python3-pip pipx
pipx ensurepath
```

Cierra y vuelve a abrir la terminal si `ansible` no aparece en el PATH. Luego:

```bash
pipx install ansible
pipx inject ansible docker
ansible-galaxy collection install community.docker
```

## Iniciar los contenedores

Desde la carpeta del proyecto:

```bash
docker compose up -d
docker ps
```

## Instalar Python en los contenedores

```bash
docker exec server1 bash -c "apt-get update && apt-get install -y python3"
docker exec server2 bash -c "apt-get update && apt-get install -y python3"
```

## Comprobar la conexión

```bash
ansible docker -i inventory.ini -m ping
```

La conexión se configura mediante el plugin `community.docker.docker`. El SDK
de Docker para Python y la colección `community.docker` deben estar instalados
en el controlador.

## Ejecutar el Playbook

```bash
ansible-playbook -i inventory.ini playbook.yml
```

## Verificar los resultados

```bash
docker exec server1 ls -R /tmp/ansible-demo
docker exec server2 ls -R /tmp/ansible-demo
docker exec server1 cat /tmp/ansible-demo/info.txt
docker exec server2 cat /tmp/ansible-demo/info.txt
```

## Evidencia de ejecución

Después de ejecutar el Playbook correctamente, guarda una captura auténtica
de la terminal donde se vean las tareas ejecutadas y el resumen `PLAY RECAP`
con `failed=0` y `unreachable=0`.

Guarda la captura en esta carpeta con el nombre `evidencia.png` y descomenta
o conserva la siguiente referencia:

![Ejecución exitosa del Playbook](evidencia.png)

> Nota: la captura no está incluida porque debe obtenerse de una ejecución real
> del laboratorio. No se deben presentar resultados simulados como evidencia.

## Estructura del proyecto

```text
ansible-lab/
├── docker-compose.yml
├── inventory.ini
├── playbook.yml
└── README.md
```
