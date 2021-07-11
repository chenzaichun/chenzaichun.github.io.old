+++
title = "高可用部署cmp相关脚本"
draft = false
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [高可用部署cmp相关脚本](#高可用部署cmp相关脚本)
    - [rightcloud\_var\_ha](#rightcloud-var-ha)
    - [rightcloud\_env\_ha](#rightcloud-env-ha)
    - [数据库初始化](#数据库初始化)
    - [注入ip相关的配置](#注入ip相关的配置)
    - [db migration](#db-migration)
    - [禁用云环境](#禁用云环境)
    - [运行](#运行)

</div>
<!--endtoc-->



## 高可用部署cmp相关脚本 {#高可用部署cmp相关脚本}

对于基础组件的部署配置，请参考高可用部署相关的文档。这里提供了2个脚本用来处理cmp相关组件的快速部署和升级的。这两个文件都放在 `/usr/local/rightcloud/` 目录下。


### rightcloud\_var\_ha {#rightcloud-var-ha}

```sh
#!/bin/bash

export PATH=$PATH:/usr/local/bin
WORKDIR=/usr/local/rightcloud

# HA configration
VIP=10.1.10.87
REDIS_HA_PORT=6378
MYSQL_HA_PORT=3406
MQ_HA_PORT=5772
MQ_MGT_HA_PORT=15772
CONSOLE_HA_PORT=13380
SERVER_HA_PORT=8444
REPOMIRROR_HA_PORT=1518
WEBSSH_HA_PORT=4833
FALCON_IP_ADDRESS=10.1.10.93
MONGODB_IP_ADDRESS=10.1.10.92
PUBLIC_IP=cmp.gsww.com
PUBLIC_IP2=202.100.86.93
CONSOLE_HA_PORT2=13381

en0="ens32"
ANYNODE_PREFIX=rightcloud
HOSTIP=`ifconfig ${en0} |grep inet |grep -v inet6 |awk '{print $2}'`
INNER_IP=`ifconfig ${en0} |grep inet |grep -v inet6 |awk '{print $2}'`

# redis related conf
REDIS_IP=${VIP}
REDIS_PORT=${REDIS_HA_PORT}
REDIS_URL=${REDIS_IP}:${REDIS_PORT}

# MQ related conf
RABBIT_MQ_URL=${VIP}
MQ_URL=${VIP}
MQ_PORT=${MQ_HA_PORT}
MQ_MGT_PORT=${MQ_MGT_PORT}
MQ_USER=admin
MQ_PASS=1

# repo mirror conf
REPOMIRROR_URI=${VIP}:${REPOMIRROR_HA_PORT}
REPOMIRROR_PORT=1517


# webssh conf
WEB_SSH_PORT=9999
GUACD_HOST=${VIP}
GUACD_PORT=${WEB_SSH_HA_PORT}


# mongodb conf
MONGODB_HOST=${MONGODB_IP_ADDRESS}
MONGODB_USERNAME=rightcloud
MONGODB_PASSWORD=H89lBgAg
MONGODB_PORT=3389

# mysql conf
MYSQL_PORT=3306
DB_URL=${VIP}:${MYSQL_HA_PORT}
DB_USER=root
DB_PASSWORD=root-mysql-pw

# monitor conf
FALCON_IP=${FALCON_IP_ADDRESS}
FALCON_DB_URL=${FALCON_IP}:3307
FALCON_DB_PASSWORD=

# server conf
SERVER_PORT=8809
SERVER_PORT_H=8080
SERVER_REST_PORT=${SERVER_PORT}

# console conf
CONSOLE_PORT=8808
UPSTREAM_SERVER=${VIP}:${SERVER_HA_PORT}
REST_URL=http://${PUBLIC_IP}
WEBSOCKET_URL=ws://${PUBLIC_IP}/api/v1/ws
WEBSSH_URL=http://${PUBLIC_IP}:${WEBSSH_HA_PORT}
UPSTREAM_DCIMSERVER=127.0.0.1
DOC_URL=http://${PUBLIC_IP}/doc/htm/docMain.html
UPLOAD_URL=http://${PUBLIC_IP}/user-console/static/files/

CONSOLE_PORT2=8108
REST_URL2=http://${PUBLIC_IP2}:${CONSOLE_HA_PORT2}
WEBSOCKET_URL2=ws://${PUBLIC_IP2}:${CONSOLE_HA_PORT2}/api/v1/ws
WEBSSH_URL2=http://${PUBLIC_IP2}:${WEBSSH_HA_PORT}
DOC_URL2=http://${PUBLIC_IP2}:${CONSOLE_HA_PORT2}/doc/htm/docMain.html


HOST_IP=${HOSTIP}
IS_GATEWAY=False
PROTOCOL=http
PLATFORM_TYPE=private
#MONGO_PORT=3389


WEBSSH_TAG=image.rightcloud.com.cn/library/rightcloud-rdp:latest
REPOMIRROR_TAG=image.rightcloud.com.cn/rightcloud/repomirror:v201901151430
ANSIBLE_TAG=image.rightcloud.com.cn/gsww-project/rightcloud-ansible:v.release-201901041846.1
SERVER_TAG=image.rightcloud.com.cn/gsww-project/rightcloud-server:v.gsww-project-201901291522.16
ADAPTER_TAG=image.rightcloud.com.cn/gsww-project/rightcloud-adapter:v.gsww-project-201901111027.3
CONSOLE_TAG=image.rightcloud.com.cn/gsww-project/rightcloud-console:v.feature-vue-201901151140.11
SCHEDULE_TAG=image.rightcloud.com.cn/gsww-project/rightcloud-schedule:v.gsww-project-201901111027.12
DB_MIGRATION_TAG=image.rightcloud.com.cn/gsww-project/rightcloud-db-migration:v.gsww-project-201901141722.13
MONGO_TAG=image.rightcloud.com.cn/library/mongodb:3.7
```

这里的修改主要是以下几个：

-   `VIP` 最终所使用的VIP
-   `FALCON_IP_ADDRESS` monitor所在机器的ip (单点)
-   `MONGODB_IP_ADDRESS` mongodb所在机器的ip (单点)
-   `PUBLIC_IP` 最终对外的IP，这里可能是VIP，也可能是其他IP（客户有自己做前置代理），新版新版本应该不需要这个了
-   `PUBLIC_IP2` 这个是给gsww用的，他们启动了2套前端，用于不同的访问
-   `en0` 这个是机器的网卡，用于自动获取IP用的


### rightcloud\_env\_ha {#rightcloud-env-ha}

```sh
#!/bin/bash

RED="\033[0;31m"
YELLOW="\033[1;33m"
GREEN="\033[0;32m"
BLUE="\033[1;34m"
LIGHT_RED="\033[1;31m"
LIGHT_GREEN="\033[1;32m"
CYAN="\033[0;36m"
LIGHT_CYAN="\033[1;36m"
WHITE="\033[1;37m"
LIGHT_GRAY="\033[0;37m"
COLOR_NONE="\e[0m"
DOCKER_LOG_OPTS="--restart=always --log-driver json-file --log-opt max-size=500m --log-opt max-file=3 "

PLATFORM=`uname -s`

if [ `uname` = 'Darwin' ]; then
    alias sed='/usr/local/bin/gsed'
    alias awk='/usr/local/bin/gawk'
fi

color_echo() {
    case "$TERM" in
        xterm*) printf "$* ${COLOR_NONE}\n"
                ;;
        *) shift; printf "$*\n"
           ;;
    esac
}

color_echo_no_newline () {
    case "$TERM" in
        xterm*) printf "$* ${COLOR_NONE}"
                ;;
        *) shift; printf "$*"
           ;;
    esac
}

error() {
    color_echo ${LIGHT_RED} $@
}

warning() {
    color_echo ${YELLOW} $@
}

logcommand() {
    color_echo_no_newline ${LIGHT_RED} "$ "
    color_echo ${GREEN} $@
}


info() {
    color_echo ${LIGHT_GREEN} $@
}


WORKDIR=/usr/local/rightcloud
. ${WORKDIR}/rightcloud_var_ha
mkdir -p /usr/local/rightcloud/files/

function valid_ip() {
    local  ip=$1
    local  stat=1

    if [[ $ip =~ ^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}$ ]]; then
        OIFS=$IFS
        IFS='.'
        ip=($ip)
        IFS=$OIFS
        [[ ${ip[0]} -le 255 && ${ip[1]} -le 255 \
               && ${ip[2]} -le 255 && ${ip[3]} -le 255 ]]
        stat=$?
    fi
    return $stat
}

function prompt_input_ip() {
    while /bin/true; do
        read -p "Please enter IP address: " -e IP

        valid_ip "${IP}"
        if [ $? -eq 0 ]; then
            break;
        else
            error "IP address is invalid, please try again"
        fi
    done

    sed -i "s/HOSTIP=.*/HOSTIP=${IP}/g" ${WORKDIR}/rightcloud_var_ha
}

function check_container ()
{
    echo "checking container: $1"
    EXISTS=`docker ps -a |awk '{print $NF}' |grep "$1" |wc -l`
    if [ $EXISTS != 0 ]; then
        echo -e "\033[36m[INFO] Remove $1...\033[0m"
        docker rm -f $1 &> /dev/null
        echo -e "\033[36m[INFO] Remove $1 success.\033[0m"
        return 1;
     else
        return 1;
     fi
}

function run_ansible() {
    check_container "${ANYNODE_PREFIX}_ansible"
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Ansible server starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -e "IS_GATEWAY=$IS_GATEWAY" \
               -e "MQ_URL=$MQ_URL" \
               -e "MQ_PORT=$MQ_PORT" \
               -e "MQ_USER=$MQ_USER" \
               -e "MQ_PASS=$MQ_PASS" \
               -e "REDIS_IP=$REDIS_IP" \
               -e "REDIS_PORT=$REDIS_PORT" \
               -e "USE_REPO_MIRROR=true" \
               -e "REPOMIRROR_URI=$REPOMIRROR_URI" \
               -e "KEEP_ORG_REPO=true" \
               -v /etc/localtime:/etc/localtime \
               --name ${ANYNODE_PREFIX}_ansible ${ANSIBLE_TAG}

        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Ansible server start failed!\033[0m";
            return 1;
        fi

        for iii in {600..0}; do
            if docker exec -i ${ANYNODE_PREFIX}_ansible ls / &> /dev/null; then
                echo "rightcloud_ansible is ready"
                break
            fi
            echo 'rightcloud_ansible init process in progress...'
            sleep 1
        done

        if [ "$iii" = 0 ]; then
            echo >&2 'rightcloud_ansible init process failed.'
            return 1
        fi

        # sleep 20

        #docker cp ${WORKDIR}/docker_install.sh rightcloud_ansible:/root/ansible_work/files/

        # docker exec ${ANYNODE_PREFIX}_ansible mkdir -p /root/ansible_work/files/jdk/
        # docker exec ${ANYNODE_PREFIX}_ansible mkdir -p /root/ansible_work/files/weblogic/
        # docker cp ${WORKDIR}/ansible_lib/jdk/* ${ANYNODE_PREFIX}_ansible:/root/ansible_work/files/jdk/
        # docker cp ${WORKDIR}/ansible_lib/weblogic/* ${ANYNODE_PREFIX}_ansible:/root/ansible_work/files/weblogic/
    fi
 }

function run_repomirror() {
    check_container ${ANYNODE_PREFIX}_repomirror
    if [ $? -ne 0 ]; then
        echo -e "\033[36m[INFO] Repo Mirror starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -p ${REPOMIRROR_PORT}:80 \
               --name ${ANYNODE_PREFIX}_repomirror ${REPOMIRROR_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Webssh start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}


function run_webssh3() {
    check_container ${ANYNODE_PREFIX}_webssh3
    if [ $? -ne 0 ]; then
       echo -e "\033[36m[INFO] Webssh starting...... \033[0m"
       docker run -d ${DOCKER_LOG_OPTS} \
              -p ${WEB_SSH_PORT}:4822 \
              --name ${ANYNODE_PREFIX}_webssh3 \
              ${WEBSSH_TAG}
       if [ $? -ne 0 ]; then
           echo -e "\033[31m[ERROR] Webssh start failed!\033[0m";
           return 1;
       fi
       sleep 3
    fi
}

function run_mongo() {
    check_container ${ANYNODE_PREFIX}_mongo
    if [ $? -ne 0 ]; then
        echo -e "\033[36m[INFO] Mongo starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -p ${MONGODB_PORT}:27017 -v ${WORKDIR}/mongo/data/mongodb:/bitnami \
        -e MONGODB_DATABASE=rightcloud \
        -e MONGODB_USERNAME=${MONGODB_USERNAME} -e MONGODB_PASSWORD=${MONGODB_PASSWORD} \
        --name ${ANYNODE_PREFIX}_mongo ${MONGO_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Mongo start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}

function run_adapter() {
    check_container ${ANYNODE_PREFIX}_adapter
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud server starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} \
               -e "JAVA_OPTS=-Xmx1024M -Xms512M -Xss2M -Djava.security.egd=file:/dev/./urandom" \
               -e "DB_USER=$DB_USER" \
               -e "DB_PASSWORD=$DB_PASSWORD" \
               -e "DB_URL=$DB_URL" \
               -e "FALCON_DB_URL=$FALCON_DB_URL" \
               -e "FALCON_DB_PASSWORD=$FALCON_DB_PASSWORD" \
               -e "RABBIT_MQ_URL=$RABBIT_MQ_URL" \
               -e "RABBIT_MQ_PORT=$MQ_PORT" \
               -e "REDIS_URL=$REDIS_URL" \
               -e "HOST_IP=$HOST_IP" \
               -e "AGENT_SERVER=$HOST_IP" \
               -e "AGENT_SERVER_PORT=$SERVER_PORT" \
               -e "FALCON_HEART_ADDR=$FALCON_IP:6030" \
               -e "FALCON_TRANSFER_ADDR=$FALCON_IP:8433" \
               -e "FALCON_TRANSFER_ADDR_HTTP=$FALCON_IP:6060" \
               -e "QUERY_HISTORY_URL=http://$FALCON_IP:9966/graph/history" \
               -e "QUERY_LAST_URL=http://$FALCON_IP:9966/graph/last" \
               -e "FALCON_FE_URL=http://$FALCON_IP:1234" \
               -e "FALCON_PORTAL_URL=http://$FALCON_IP:5050" \
               -e "PORTAL_URL=$PORTAL_URL" \
               -e "COOKIE_DOMAIN=$COOKIE_DOMAIN" \
               -v /etc/localtime:/etc/localtime \
               -e "PLATFORM_TYPE=$PLATFORM_TYPE" \
               -e "MONGODB_HOST=$MONGODB_HOST" \
               -e "MONGODB_USERNAME=$MONGODB_USERNAME" \
               -e "MONGODB_PASSWORD=$MONGODB_PASSWORD" \
               -e "MONGODB_PORT=$MONGODB_PORT" \
               --name ${ANYNODE_PREFIX}_adapter \
               ${ADAPTER_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud server start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}

function run_dbmigration() {
    docker run --rm \
           $DB_IMAGE_TAG \
           -outOfOrder=true \
           -ignoreMissingMigrations=true \
           -placeholderReplacement=true \
           -placeholderPrefix=#[ \
                                 -placeholderSuffix=] \
           -table=schema_version \
           -url="jdbc:mysql://${DB_URL}/rightcloud?useUnicode=true&characterEncoding=utf-8&useSSL=false" -schemas=rightcloud -user="${DB_USER}" -password="$DB_PASSWORD" migrate
}

function run_console() {

    check_container ${ANYNODE_PREFIX}_console
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud console starting...... \033[0m"
        # docker run -d ${DOCKER_LOG_OPTS} -p ${CONSOLE_PORT}:80 \
        #        -e CONSOLE_ADDRESS=${CONSOLE_ADDRESS} \
        #        -e CONSOLE_PORT=${CONSOLE_PORT} \
        #        -e PROTOCOL=${PROTOCOL} \
        #        -e WEB_SSH_PORT=${WEB_SSH_PORT} \
        #        -e UPSTREAM_SERVER=${UPSTREAM_SERVER} \
        #        -e COOKIE_DOMAIN=${COOKIE_DOMAIN} \
        #        -e PLATFORM_TYPE=private \
        #        -e REST_URL=${REST_URL} \
        #        -e WEBSOCKET_URL=${WEBSOCKET_URL} \
        #        -e UPSTREAM_DCIMSERVER=${UPSTREAM_DCIMSERVER} \
        #        -e DOC_URL=${DOC_URL} \
        #        -v /etc/localtime:/etc/localtime \
        #        -v /usr/local/rightcloud/files:/usr/share/nginx/html/user-console/static/files \
        #        --name ${ANYNODE_PREFIX}_console \
        #        ${CONSOLE_TAG}

        docker run -d ${DOCKER_LOG_OPTS} -p ${CONSOLE_PORT}:80 \
                   -e UPSTREAM_SERVER=${UPSTREAM_SERVER} \
                   -e REST_URL=${REST_URL} \
                   -e WEBSOCKET_URL=${WEBSOCKET_URL} \
                   -e UPSTREAM_DCIMSERVER=${UPSTREAM_DCIMSERVER} \
                   -e DOC_URL=${DOC_URL} \
                   -e WEBSSH_URL=${WEBSSH_URL} \
                   -v /etc/localtime:/etc/localtime \
                   -v /usr/local/rightcloud/nfs/files:/usr/share/nginx/html/user-console/static/files \
                   --name ${ANYNODE_PREFIX}_console \
                   ${CONSOLE_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud console start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}


function run_console2() {

    check_container ${ANYNODE_PREFIX}_console2
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud console starting...... \033[0m"
        # docker run -d ${DOCKER_LOG_OPTS} -p ${CONSOLE_PORT}:80 \
        #        -e CONSOLE_ADDRESS=${CONSOLE_ADDRESS} \
        #        -e CONSOLE_PORT=${CONSOLE_PORT} \
        #        -e PROTOCOL=${PROTOCOL} \
        #        -e WEB_SSH_PORT=${WEB_SSH_PORT} \
        #        -e UPSTREAM_SERVER=${UPSTREAM_SERVER} \
        #        -e COOKIE_DOMAIN=${COOKIE_DOMAIN} \
        #        -e PLATFORM_TYPE=private \
        #        -e REST_URL=${REST_URL} \
        #        -e WEBSOCKET_URL=${WEBSOCKET_URL} \
        #        -e UPSTREAM_DCIMSERVER=${UPSTREAM_DCIMSERVER} \
        #        -e DOC_URL=${DOC_URL} \
        #        -v /etc/localtime:/etc/localtime \
        #        -v /usr/local/rightcloud/files:/usr/share/nginx/html/user-console/static/files \
        #        --name ${ANYNODE_PREFIX}_console \
        #        ${CONSOLE_TAG}

        docker run -d ${DOCKER_LOG_OPTS} -p ${CONSOLE_PORT2}:80 \
                   -e UPSTREAM_SERVER=${UPSTREAM_SERVER} \
                   -e REST_URL=${REST_URL2} \
                   -e WEBSOCKET_URL=${WEBSOCKET_URL2} \
                   -e UPSTREAM_DCIMSERVER=${UPSTREAM_DCIMSERVER} \
                   -e DOC_URL=${DOC_URL2} \
                   -e WEBSSH_URL=${WEBSSH_URL2} \
                   -v /etc/localtime:/etc/localtime \
                   -v /usr/local/rightcloud/nfs/files:/usr/share/nginx/html/user-console/static/files \
                   --name ${ANYNODE_PREFIX}_console2 \
                   ${CONSOLE_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud console start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}


function run_server() {

    check_container ${ANYNODE_PREFIX}_server
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud server starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} \
               -p ${SERVER_PORT_H}:8080 \
               -p ${SERVER_PORT}:8443 \
               -p ${WEB_SSH_PORT}:4200 \
               -e "DB_USER=$DB_USER" \
               -e "DB_PASSWORD=$DB_PASSWORD" \
               -e "JAVA_OPTS=-Xmx1024M -Xms512M -Xss2M -Djava.security.egd=file:/dev/./urandom" \
               -e "DB_URL=$DB_URL" \
               -e "FALCON_DB_URL=$FALCON_DB_URL" \
               -e "FALCON_DB_PASSWORD=$FALCON_DB_PASSWORD" \
               -e "FALCON_DB_USER=$DB_USER" \
               -e "RABBIT_MQ_URL=$RABBIT_MQ_URL" \
               -e "RABBIT_MQ_PORT=$MQ_PORT" \
               -e "REDIS_URL=$REDIS_URL" \
               -e "HOST_IP=$HOST_IP" \
               -e "AGENT_SERVER=$VIP" \
               -e "AGENT_SERVER_PORT=$SERVER_PORT" \
               -e "FALCON_HEART_ADDR=$FALCON_IP:6030" \
               -e "FALCON_TRANSFER_ADDR=$FALCON_IP:8433" \
               -e "FALCON_TRANSFER_ADDR_HTTP=$FALCON_IP:6060" \
               -e "QUERY_HISTORY_URL=http://$FALCON_IP:9966/graph/history" \
               -e "QUERY_LAST_URL=http://$FALCON_IP:9966/graph/last" \
               -e "FALCON_FE_URL=http://$FALCON_IP:1234" \
               -e "FALCON_PORTAL_URL=http://$FALCON_IP:5050" \
               -e "PORTAL_URL=$PORTAL_URL" \
               -e "COOKIE_DOMAIN=$COOKIE_DOMAIN" \
               -e "PLATFORM_TYPE=$PLATFORM_TYPE" \
               -e "MONGODB_HOST=$MONGODB_HOST" \
               -e "MONGODB_USERNAME=$MONGODB_USERNAME" \
               -e "MONGODB_PASSWORD=$MONGODB_PASSWORD" \
               -e "MONGODB_PORT=$MONGODB_PORT" \
               -e "GUACD_HOST=${GUACD_HOST}" \
               -e "GUACD_PORT=${GUACD_PORT}" \
               -e "HA_DEPLOY=true" \
               -v /etc/localtime:/etc/localtime \
               -v /usr/local/rightcloud/nfs/files:/opt/rightcloud/files \
               --name ${ANYNODE_PREFIX}_server \
               ${SERVER_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud server start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}

function run_schedule() {
  #statements
  check_container ${ANYNODE_PREFIX}_schedule
  if [ $? = 1 ]; then
      echo -e "\033[36m[INFO] Rightcloud schedule starting...... \033[0m"
      docker run -d ${DOCKER_LOG_OPTS} -e "DB_HOST=${HOST_IP}" \
             -e "DB_PORT=${MYSQL_PORT}" \
             -e "DB_USERNAME=$DB_USER" \
             -e "DB_PASSWORD=$DB_PASSWORD" \
             -e "REDIS_HOST=${REDIS_IP} " \
             -e "REDIS_PORT=${REDIS_PORT} " \
             -e "MQ_HOST=${MQ_URL} " \
             -e "MQ_PORT=${MQ_PORT}" \
             -e "MQ_USERNAME=${MQ_USER}" \
             -e "MQ_PASSWORD=${MQ_PASS}" \
             -e "MONGODB_HOST=${MONGODB_HOST}" \
             -e "MONGODB_USERNAME=${MONGODB_USERNAME}" \
             -e "MONGODB_PASSWORD=${MONGODB_PASSWORD}" \
             -e "MONGODB_PORT=${MONGODB_PORT}" \
             -e "PROFILES_ACTIVE=cloudstar" \
             -v "/etc/localtime:/etc/localtime" \
             --name ${ANYNODE_PREFIX}_schedule \
             ${SCHEDULE_TAG}
      if [ $? -ne 0 ]; then
          echo -e "\033[31m[ERROR] Rightcloud portal start failed!\033[0m";
          return 1;
      fi
      sleep 3
  fi
}

function run_viclient() {
    #statements
    check_container ${ANYNODE_PREFIX}_viclient
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud vi-client starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} \
               -e "MQ_HOST=${MQ_URL} " \
               -e "MQ_PORT=${MQ_PORT}" \
               -e "MQ_USERNAME=${MQ_USER}" \
               -e "MQ_PASSWORD=${MQ_PASS}" \
               -e "PROFILES_ACTIVE=cloudstar" \
               -v "/etc/localtime:/etc/localtime" \
               --name ${ANYNODE_PREFIX}_viclient \
               ${VICLIENT_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud vi-client start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}


function run_portal() {
    check_container ${ANYNODE_PREFIX}_portal
    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud portal starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -p ${ANYNODE_PORTAL_PORT}:80  \
               -e "CONSOLE_ADDRESS=$CONSOLE_ADDRESS" \
               -e "CONSOLE_PORT=$CONSOLE_PORT" \
               -e "COOKIE_DOMAIN=$COOKIE_DOMAIN" \
               -e "PROTOCOL=$PROTOCOL" \
               -e "UPSTREAM_SERVER=$UPSTREAM_SERVER" \
               -v /etc/localtime:/etc/localtime \
               --name ${ANYNODE_PREFIX}_portal \
               ${PORTAL_TAG}
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud portal start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}

gen_sql_update() {
    cat > ${WORKDIR=}/b1.sql <<EOF
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\`='http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/html/user/email-change-validate.html'  WHERE \`CONFIG_KEY\`='email.change.validate';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\`='http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/'  WHERE \`CONFIG_KEY\`='portal.url';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\`='http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/html/user/initpwd-reset.html'  WHERE \`CONFIG_KEY\`='server.account.active.url';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\`='http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/html/user/resetpwd-reset.html'  WHERE \`CONFIG_KEY\`='mail.account.change.pwd.url';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\`='http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/login.html'  WHERE \`CONFIG_KEY\`='console.login.url';
UPDATE \`rightcloud\`.\`script_param_config\` SET \`default_value\`='${FALCON_IP_ADDRESS}:6060'  WHERE \`name\`='open_falcon_transfer_addr_http';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`config_value\` = '${VIP}:${MQ_HA_PORT}' WHERE \`config_key\` = 'rclink.mq.server';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\` = 'http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/resetpwd-reset.html'  WHERE \`CONFIG_KEY\` = 'mail.account.change.pwd.url';
UPDATE \`rightcloud\`.\`sys_m_config\` SET \`CONFIG_VALUE\` = 'http://${PUBLIC_IP}:${CONSOLE_HA_PORT}/api/v1/alarms/falcon/callback'  WHERE \`CONFIG_KEY\` = 'openfalcon.alarm.callback.url';
EOF
}

disable_qingniu() {
    cat > ${WORKDIR}/b2.sql <<EOF
UPDATE `sys_m_code` SET `ENABLED`='0' WHERE (`CODE_SID`='5012');
UPDATE `sys_m_code` SET `ENABLED`='0' WHERE (`CODE_SID`='6248');
UPDATE `sys_m_code` SET `ATTRIBUTE_2`='0' WHERE (`CODE_SID`='5187');
UPDATE `sys_m_code` SET `ATTRIBUTE_2`='0' WHERE (`CODE_SID`='6249');
EOF
}


function run_mysql() {
    check_container ${ANYNODE_PREFIX}_mysql

    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud mysql starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -p ${MYSQL_PORT}:3306  -v ${WORKDIR}/data:/var/lib/mysql  -e MYSQL_ROOT_PASSWORD=root-mysql-pw  --name ${ANYNODE_PREFIX}_mysql  -v /etc/localtime:/etc/localtime image.rightcloud.com.cn/library/mysql:5.7.20
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud mysql start failed!\033[0m";
            return 1;
        fi

        mysql=( mysql -uroot -h${HOSTIP} -proot-mysql-pw )

        for i in {600..0}; do
            if echo 'SELECT 1' | "${mysql[@]}" &> /dev/null; then
                echo "MySQL is ready"
                break
            fi
            echo 'MySQL init process in progress...'
            sleep 1
        done

        if [ "$i" = 0 ]; then
            echo >&2 'MySQL init process failed.'
            return 1
        fi

        # sleep 60
        gen_sql_update
        docker cp ${WORKDIR}/b1.sql ${ANYNODE_PREFIX}_mysql:/root/
        docker exec ${ANYNODE_PREFIX}_mysql sh -c "mysql -uroot -proot-mysql-pw rightcloud < /root/b1.sql"
    fi
}

function run_rabbitmq() {
    check_container ${ANYNODE_PREFIX}_rabbitmq

    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud rabbitmq starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -p 5671:5171 -p ${MQ_PORT}:5672 -p 15671:15671 -p ${MQ_MGT_PORT}:15672 --name ${ANYNODE_PREFIX}_rabbitmq image.rightcloud.com.cn/library/rabbitmq:3.5.6-management
        if [ $? -gt 1 ]; then
            echo -e "\033[31m[ERROR] Rightcloud rabbitmq start failed!\033[0m";
            return 1;
        fi

        for ii in {600..0}; do
            if docker exec -i ${ANYNODE_PREFIX}_rabbitmq rabbitmqctl cluster_status &> /dev/null; then
                echo "RabbitMQ is ready"
                break
            fi
            echo 'RabbitMQ init process in progress...'
            sleep 1
        done

        if [ "$ii" = 0 ]; then
            echo >&2 'RabbitMQ init process failed.'
            return 1
        fi

        # sleep 60

        docker exec ${ANYNODE_PREFIX}_rabbitmq rabbitmqctl add_user admin 1
        docker exec ${ANYNODE_PREFIX}_rabbitmq rabbitmqctl set_user_tags admin administrator
        docker exec ${ANYNODE_PREFIX}_rabbitmq rabbitmqctl set_permissions admin ".*" ".*" ".*"
        sleep 3
    fi
}

run_haproxy() {
    check_container ${ANYNODE_PREFIX}_haproxy

    if [ $? = 1 ]; then

    docker run -d  ${DOCKER_LOG_OPTS} \
           -p ${MYSQL_HA_PORT}:3306 \
           -p ${MQ_HA_PORT}:5672 \
           -p ${REDIS_HA_PORT}:6379 \
           -p ${MQ_MGT_HA_PORT}:15672 \
           -p ${CONSOLE_HA_PORT}:80 \
           -p ${CONSOLE_HA_PORT2}:81 \
           -p ${SERVER_HA_PORT}:8443 \
           -p ${REPOMIRROR_HA_PORT}:${REPOMIRROR_HA_PORT} \
           -p ${WEBSSH_HA_PORT}:4200 \
           -p 1080:1080 \
           -v /opt/haproxy:/usr/local/etc/haproxy \
           --name ${ANYNODE_PREFIX}_haproxy \
           image.rightcloud.com.cn/library/haproxy:1.8
    fi

}

run_keepalived() {
    check_container ${ANYNODE_PREFIX}_keepalived_master

    if [ $? = 1 ]; then

    docker run ${DOCKER_LOG_OPTS} --cap-add=NET_ADMIN \
           --net=host --privileged -d \
           -v /opt/keepalived/keepalived.conf:/container/service/keepalived/assets/keepalived.conf \
           -v /opt/keepalived/check_nginx.sh:/etc/keepalived/check_nginx.sh \
           --name ${ANYNODE_PREFIX}_keepalived_master \
           image.rightcloud.com.cn/library/keepalived:1.3.9 --copy-service
    fi

}

run_keepalive_slave() {
    check_container ${ANYNODE_PREFIX}_keepalived_slave

    if [ $? = 1 ]; then


    docker run ${DOCKER_LOG_OPTS} --cap-add=NET_ADMIN \
           --net=host --privileged -d \
           -v /opt/keepalived/keepalived.conf:/container/service/keepalived/assets/keepalived.conf \
           -v /opt/keepalived/check_nginx.sh:/etc/keepalived/check_nginx.sh \
           --name ${ANYNODE_PREFIX}_keepalived_slave \
           image.rightcloud.com.cn/library/keepalived:1.3.9 --copy-service

    fi
}

function run_redis() {
    check_container ${ANYNODE_PREFIX}_redis

    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud redis starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} -p ${REDIS_PORT}:6379 --name ${ANYNODE_PREFIX}_redis image.rightcloud.com.cn/library/redis:latest
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud redis start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}

function run_monitor() {
    check_container ${ANYNODE_PREFIX}_monitor

    if [ $? = 1 ]; then
        echo -e "\033[36m[INFO] Rightcloud monitor starting...... \033[0m"
        docker run -d ${DOCKER_LOG_OPTS} \
               -p 1234:1234 \
               -p 5050:5050 \
               -p 9912:9912 \
               -p 6030:6030 \
               -p 6060:6060 \
               -p 9966:9966 \
               -p 8433:8433 \
               -p 8081:8081 \
               -p 3307:3306 \
               -v /etc/localtime:/etc/localtime \
               -e "HOST_IP=${HOSTIP}" \
               --name ${ANYNODE_PREFIX}_monitor image.rightcloud.com.cn/rightcloud/rightcloud-monitor:v1.0
        if [ $? -ne 0 ]; then
            echo -e "\033[31m[ERROR] Rightcloud monitor start failed!\033[0m";
            return 1;
        fi
        sleep 3
    fi
}

function run_harbor() {
    sed -i "s/hostname =.*/hostname = ${HOSTIP}/g" /opt/harbor/harbor.cfg
    docker-compose -f /opt/harbor/docker-compose.yml up -d
}

addport() {
    ports=(3307 6379 6060 6030 9966 9999 8080 80 8081 8433 1234 8501 15671 9912 5050 3389 5671 8808 8809 1514 3306 3690)
    for p in ${ports[@]}; do
        firewall-cmd --add-port=${p}/tcp --zone=public
        firewall-cmd --permanent --add-port=${p}/tcp --zone=public
    done
}

removeport() {
    ports=(3307 6379 6060 6030 9966 9999 8080 80 8081 8433 1234 8501 15671 9912 5050 3389 5671 8808 8809 1514 3306 3690)
    for p in ${ports[@]}; do
        firewall-cmd --permanent --remove-port=${p}/tcp --zone=public
    done
}

deploy-server() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the server_tag"
        return 1
    fi
    sed -i "s!SERVER_TAG=.*!SERVER_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_server
}

deploy-portal() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the portal_tag"
        return 1
    fi
    sed -i "s!PORTAL_TAG=.*!PORTAL_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_portal
}

deploy-console() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the console_tag"
        return 1
    fi
    sed -i "s!CONSOLE_TAG=.*!CONSOLE_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_console
}

deploy-schedule() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the schedule_tag"
        return 1
    fi
    sed -i "s!SCHEDULE_TAG=.*!SCHEDULE_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_schedule
}

deploy-ansible() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the ansible_tag"
        return 1
    fi
    sed -i "s!ANSIBLE_TAG=.*!ANSIBLE_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_ansible
}

deploy-adapter() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the adapter_tag"
        return 1
    fi
    sed -i "s!ADAPTER_TAG=.*!ADAPTER_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_adapter
}

deploy-dbmigration() {
    if [ "x$1" = "x" ]; then
        echo "Please specify the db_migration_tag"
        return 1
    fi
    sed -i "s!DB_MIGRATION_TAG=.*!DB_MIGRATION_TAG=$1!g" ${WORKDIR}/rightcloud_var_ha
    . ${WORKDIR}/rightcloud_var_ha
    run_dbmigration
}

check_container_status() {
    if [ "x$1" = "x" ]; then
        echo "Please provide container_name for checking"
        return
    fi

    RUNNING=`docker ps -q|xargs docker inspect -f '{{.Name}}'|grep $1 |grep -v grep|wc -l|tr -d " "`

    if [ $RUNNING != 0 ]; then
        echo -e "$1 is\033[1;32m running \033[0m"
    else
        echo -e "$1 is\033[1;31m not running \033[0m"
    fi
}


rightcloud_start(){
    #prompt_input_ip
    echo 'starting rightcloud service...'
    . /usr/local/rightcloud/rightcloud_env
    systemctl start docker
    run_repomirror
    run_harbor
    run_rabbitmq
    run_mysql
    run_redis
    run_mongo
    run_monitor
    run_webssh3
    #run_portal
    run_server
    run_adapter
    run_ansible
    run_console
    run_schedule
}

rightcloud_stop(){
    echo 'stopping rightcloud service...'
    docker ps -a|xargs docker kill &> /dev/null
    #cd /opt/harbor && docker-compose stop
    #docker ps -q|xargs docker kill &> /dev/null
}

rightcloud_restart() {
    echo "restarting rightlcoud service..."
    rightcloud_stop
    sleep 10
    rightcloud_start
}

rightcloud_status() {
    . /usr/local/rightcloud/rightcloud_env
    check_container_status "rightcloud_mysql"
    check_container_status "rightcloud_redis"
    check_container_status "rightcloud_rabbitmq"
    check_container_status "rightcloud_monitor"
    check_container_status "rightcloud_webssh3"
    check_container_status "rightcloud_mongo"
    check_container_status "rightcloud_server"
    check_container_status "rightcloud_adapter"
    check_container_status "rightcloud_ansible"
    check_container_status "rightcloud_repomirror"
    check_container_status "rightcloud_console"
    check_container_status "rightcloud_schedule"
    check_container_status "harbor-jobservice"
    check_container_status "harbor-db"
    check_container_status "registry"
    check_container_status "harbor-ui"
    check_container_status "harbor-log"
    check_container_status "nginx"
}
```


### 数据库初始化 {#数据库初始化}

首先需要拿到db base的sql，这个找相关人拿，拿到之后导入进去：

创建数据库，权限自行控制。

```sql
create database if not exists rightcloud character set UTF8mb4 collate utf8mb4_bin;
ALTER USER 'root'@'%' IDENTIFIED BY 'root-mysql-pw';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root-mysql-pw';
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root-mysql-pw';
```

导入数据，假如这里的处初始化数据为 `rightcloud.sql`

```sh
mysql -uroot -h xxxxxx -p rightcloud < rightcloud.sql
```


### 注入ip相关的配置 {#注入ip相关的配置}

首先需要 `source` 一下 `rightcloud_env_ha` 这个文件

```sh
. /usr/local/rightcloud_env_ha
```

```sh
gen_sql_update
```

这里会在当前目录生成 `b1.sql`, 导入

```sh
mysql -uroot -h xxxxxx -p rightcloud < b1.sql
```


### db migration {#db-migration}

首先需要 `source` 一下 `rightcloud_env_ha` 这个文件

```sh
. /usr/local/rightcloud_env_ha
```

然后做dbmigration

```sh
deploy-dbmigration db-migration-tag
```


### 禁用云环境 {#禁用云环境}

这个找相关的人要sql


### 运行 {#运行}

首先需要 `source` 一下 `rightcloud_env_ha` 这个文件

```sh
. /usr/local/rightcloud_env_ha
```

然后就可以运行相应的组件了:

```sh
run_server
run_adapter
...
```

如果需要升级一下对应的组件：

```sh
deploy-server server_docker_image_tag
...
```
