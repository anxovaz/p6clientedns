<h1>Práctica 6 - Anxo Vázquez</h1>
<h2>Usa unha imaxe alpine</h2>
<p>Para descargar a imaxe alpine a descargo con "docker pull imaxe"</p>
<p>docker pull alpine</p>
<br>
<h2>Descargo docker-compose.yml</h2>
<p>Descargo todo o repositorio con "git clone" e executo "docker compose up"</p>
<p>git clone https://github.com/damiancastelao/practica01-DNS</p>
<br>
<h2>Crear rede "bind9_subnet"</h2>
<p>Creo a rede bind9_subnet establecendo a subrede con 172.28.0.0/16 co rango 172.28.5.0/24</p>
<p>docker network create \
  --driver=bridge \
  --subnet=172.28.0.0/16 \
  --ip-range=172.28.5.0/24 \
  --gateway=172.28.5.254 \
  bind9_subnet</p>
<br>
<h2>Solución erro docker compose up</h2>
<p>Ao executar "docker compose up" da erro porque o porto 53 está en uso, como esta sendo usado por un servisio do sistema, cambio o archivo docker-compose.yml e cambio os portos a 120:120</p>
<p>ports:
      - 120:120
</p>
<br>
<h2>Configuración cliente</h2>
<p>Despois de executar "docker compose up" métome no cliente con "docker exec" e instalo os paquetes necesarios</p>
<p>docker compose up</p>
<p>docker exec -it cliente /bin/bash</p>
<p>apt update</p>
<p>apt install iputils-ping</p>
<p>apt install -y dnsutils</p>
<br>
<h2>Proba</h2>
<p>Probo a facer ping a un nome de dominio, exemplo: google.es</p>
<p>ping google.es</p>
<p>dig google.es</p>

<h2>Parámetros .yml<h2>
<p>services:</p>
<p>#SERVIDOR</p>
<p>  asir_bind9:</p>
<p>    container_name: asir_bind9</p>
<p>    image: ubuntu/bind9 #imaxe</p>
<p>    platform: linux/amd64</p>
<p>    ports: #portos para as consultas dns</p>
<p>      - 120:120</p>
<p>    networks: #rede</p>
<p>      bind9_subnet:</p>
<p>        ipv4_address: 172.28.5.1 #ip asir_bind9</p>
<p>    volumes:</p>
<p>      - ./conf:/etc/bind #monta o contido de ./conf en /etc/bind</p>
<p>      - ./zonas:/var/lib/bind #monta o contido de ./zonas en /var/lib/bind</p>
<p>#CLIENTE</p>
<p>  cliente:</p>
<p>    container_name: cliente</p>
<p>    image: ubuntu #imaxe</p>
<p>    platform: linux/amd64</p>
<p>    tty: true</p>
<p>    stdin_open: true</p>
<p>    dns:</p>
<p>      - 172.28.5.1 #especifico a ip do servidor asir_bind9 para que resolva as consultas</p>
<p>    networks:</p>
<p>      bind9_subnet:</p>
<p>        ipv4_address: 172.28.5.2 #ip cliente</p>
<p>networks: #configuracion rede</p>
<p>  bind9_subnet:</p>
<p>    external: true</p>


