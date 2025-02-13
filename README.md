paso 1 
sudo apt install ca-certificates curl gnupg
paso 2
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt remove $pkg; done

paso 3
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
paso 4
sudo usermod -aG docker $USER && su $USER
export DOWNLOAD_DIR=$HOME/greenbone-community-container && mkdir -p $DOWNLOAD_DIR
paso 5
curl -f -O -L https://greenbone.github.io/docs/latest/_static/docker-compose.yml --output-dir "$DOWNLOAD_DIR"
docker compose -f $DOWNLOAD_DIR/docker-compose.yml pull
termina 
paso 6
docker compose -f $DOWNLOAD_DIR/docker-compose.yml up -d

paso 7
docker compose -f $DOWNLOAD_DIR/docker-compose.yml logs -f

pass admin:admin
(Cambiar pass)
docker compose -f $DOWNLOAD_DIR/docker-compose.yml \
    exec -u gvmd gvmd gvmd --user=admin --new-password='ADMIN123$'
paso 8
xdg-open "http://127.0.0.1:9392" 2>/dev/null >/dev/null &

paso #9
curl -f -O https://greenbone.github.io/docs/latest/_static/setup-and-start-greenbone-community-edition.sh && chmod u+x setup-and-start-greenbone-community-edition.sh

#
./setup-and-start-greenbone-community-edition.sh

#####Actualizar feed#####
docker compose -f $DOWNLOAD_DIR/docker-compose.yml pull
docker compose -f $DOWNLOAD_DIR/docker-compose.yml up -d
#
docker compose -f $DOWNLOAD_DIR/docker-compose.yml pull notus-data vulnerability-tests scap-data dfn-cert-data cert-bund-data report-formats data-objects
#
docker compose -f $DOWNLOAD_DIR/docker-compose.yml up -d notus-data vulnerability-tests scap-data dfn-cert-data cert-bund-data report-formats data-objects

"##
docker compose -f $DOWNLOAD_DIR/docker-compose.yml down -v
$#####
docker compose -f $DOWNLOAD_DIR/docker-compose.yml exec <container-name> /bin/bash
gvm-cli --gmp-username admin socket --pretty --xml "<get_version/>"




