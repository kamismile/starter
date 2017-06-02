[TOC]

# binlog

设置binlog及相关参数可以通过启动mysqld时传入相应的启动参数设置，也可以通过系统参数配置文件设置

https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#option_mysqld_log-bin



# mysql master-slave



```dockerfile
FROM mysql:5.7
MAINTAINER Tao Wang <twang2218@gmail.com>
COPY replica.sh /docker-entrypoint-initdb.d/
```

```dockerfile
version: '2'
services:
    master:
        image: twang2218/mysql:5.7-replica
        build: .
        restart: unless-stopped
        ports:
            - 3306:3306
        environment:
            - MYSQL_ROOT_PASSWORD=master_passw0rd
            - MYSQL_REPLICA_USER=replica
            - MYSQL_REPLICA_PASS=replica_Passw0rd
        command: ["mysqld", "--log-bin=mysql-bin", "--server-id=1"]
    slave:
        image: twang2218/mysql:5.7-replica
        build: .
        restart: unless-stopped
        ports:
            - 3307:3306
        environment:
            - MYSQL_ROOT_PASSWORD=slave_passw0rd
            - MYSQL_REPLICA_USER=replica
            - MYSQL_REPLICA_PASS=replica_Passw0rd
            - MYSQL_MASTER_SERVER=master
            - MYSQL_MASTER_WAIT_TIME=10
        command: ["mysqld", "--log-bin=mysql-bin", "--server-id=2"]
```

```shell
#!/bin/bash

# Most of the code are copied from ioggstream's PR, docker-library/mysql#43:
# <https://github.com/docker-library/mysql/pull/43/files>
#
# Supported environment variables for this replica.sh:
#  - MYSQL_REPLICA_USER: create the given user on the intended master host
#  - MYSQL_REPLICA_PASS
#  - MYSQL_MASTER_SERVER: change master on this location on the intended slave
#  - MYSQL_MASTER_PORT: optional, by default 3306
#  - MYSQL_MASTER_WAIT_TIME: seconds to wait for the master to come up

#
# A replication user (actually created on both master and slaves)
#
if [ "$MYSQL_REPLICA_USER" ]; then
        if [ -z "$MYSQL_REPLICA_PASS" ]; then
                echo >&2 'error: MYSQL_REPLICA_USER set, but MYSQL_REPLICA_PASS not set'
                exit 1
        fi
        echo "CREATE USER '$MYSQL_REPLICA_USER'@'%' IDENTIFIED BY '$MYSQL_REPLICA_PASS'; " | "${mysql[@]}"
        echo "GRANT REPLICATION SLAVE ON *.* TO '$MYSQL_REPLICA_USER'@'%'; " | "${mysql[@]}"
        # REPLICATION CLIENT privileges are required to get master position
        echo "GRANT REPLICATION CLIENT ON *.* TO '$MYSQL_REPLICA_USER'@'%'; " | "${mysql[@]}"
fi

#
# On the slave: point to a master server
#
if [ "$MYSQL_MASTER_SERVER" ]; then
    MYSQL_MASTER_PORT=${MYSQL_MASTER_PORT:-3306}
    MYSQL_MASTER_WAIT_TIME=${MYSQL_MASTER_WAIT_TIME:-3}

    if [ -z "$MYSQL_REPLICA_USER" ]; then
            echo >&2 'error: MYSQL_REPLICA_USER not set'
            exit 1
    fi
    if [ -z "$MYSQL_REPLICA_PASS" ]; then
            echo >&2 'error: MYSQL_REPLICA_PASS not set'
            exit 1
    fi

    # Wait for eg. 10 seconds for the master to come up
    # do at least one iteration
    for i in $(seq $((MYSQL_MASTER_WAIT_TIME + 1))); do
        if ! mysql "-u$MYSQL_REPLICA_USER" "-p$MYSQL_REPLICA_PASS" "-h$MYSQL_MASTER_SERVER" -e 'select 1;' |grep -q 1; then
            echo >&2 "Waiting for $MYSQL_REPLICA_USER@$MYSQL_MASTER_SERVER"
            sleep 1
        else
            break
        fi
    done

    if [ "$i" -gt "$MYSQL_MASTER_WAIT_TIME" ]; then
        echo 2>&1 "Master is not reachable"
        exit 1
    fi

    # Get master position and set it on the slave. NB: MASTER_PORT and MASTER_LOG_POS must not be quoted
    MasterPosition=$(mysql "-u$MYSQL_REPLICA_USER" "-p$MYSQL_REPLICA_PASS" "-h$MYSQL_MASTER_SERVER" -e "show master status \G" | awk '/Position/ {print $2}')
    MasterFile=$(mysql  "-u$MYSQL_REPLICA_USER" "-p$MYSQL_REPLICA_PASS" "-h$MYSQL_MASTER_SERVER"   -e "show master status \G"     | awk '/File/ {print $2}')
    echo "CHANGE MASTER TO MASTER_HOST='$MYSQL_MASTER_SERVER', MASTER_PORT=$MYSQL_MASTER_PORT, MASTER_USER='$MYSQL_REPLICA_USER', MASTER_PASSWORD='$MYSQL_REPLICA_PASS', MASTER_LOG_FILE='$MasterFile', MASTER_LOG_POS=$MasterPosition;"  | "${mysql[@]}"

    echo "START SLAVE;"  | "${mysql[@]}"
fi
```

