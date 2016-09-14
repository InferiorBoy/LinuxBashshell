#!/bin/bash
# chkconfig: - 85 15
# description: Nginx server control script
# processname: nginx
# config file: /usr/local/nginx/conf/nginx.conf
# pid file: /usr/local/nginx/logs/nginx.pid
# 
# eastmoney public tools
# version: v1.0.0
# create by XuHoo, 2016-9-14
# 

# source function library
. /etc/rc.d/init.d/functions

NGINX_NAME="nginx"
NGINX_PROG="/usr/local/sbin/nginx"
NGINX_PID_FILE="/usr/local/nginx/logs/nginx.pid"
NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"
NGINX_LOCK_FILE="/var/lock/subsys/nginx.lock"

# check current user
[ "$USER" != "root" ] && exit 1

start() {
    status
        if [[ $? -eq 0 ]]; then
            echo $"Nginx (PID $(cat $NGINX_PID_FILE)) already started."
            return 1
        fi
    echo -n $"Starting $NGINX_NAME: "
        daemon $NGINX_PROG -c $NGINX_CONF_FILE
        echo
        retval=$?
    [ $retval -eq 0 ] && touch $NGINX_LOCK_FILE
    return $retval
}


stop() {
    status
        if [[ $? -eq 1 ]]; then
            echo "Nginx server already stopped."
            return 1
        fi
    echo -n $"Stoping $NGINX_NAME: "
        killproc $NGINX_PROG
        echo
        retval=$?
    [ $retval -eq 0 ] && rm -f $NGINX_LOCK_FILE
    return $retval
}


restart() {
    stop
        sleep 1
    start
    retval=$?
    return $retval
}


reload() {
    echo -n $"Reloading $NGINX_NAME: "
        killproc $NGINX_PROG -HUP
        echo
        retval=$?
    return $retval
}


status() {
    netstat -anpt | grep "/nginx" | awk '{print $6}' &> /dev/null
        if [[ $? -eq 0 ]]; then
            if [[ -f $NGINX_LOCK_FILE ]]; then
                return 0
            else
                return 1
            fi
        fi
    return 1
}


_status() {
    status
        if [[ $? -eq 0 ]]; then
            state=`netstat -anpt | grep "/nginx" | awk '{ print $6 }'`
            echo $"Nginx server status is: $state"
        else
            echo "Nginx server is not running"
        fi
}


test() {
    $NGINX_PROG -t -c $NGINX_CONF_FILE
        retval=$?
    return $retval
}


case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    reload)
        reload
        ;;
    restart)
        restart
        ;;
    status)
        _status
        ;;
    test)
        test
        ;;
    *)
        echo "Usage: { start | stop | reload | restart | status | test }"
        exit 1
esac
