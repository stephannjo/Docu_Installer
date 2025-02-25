# RabbitMQ Installation and Configuration Scripts

## rmq-main-installer-silent.bat
```batch
echo.
echo This script installs the following packages:
echo Python, Erlang/OTP, and RabbitMQ.
echo.
rem echo Press any key to continue, [Ctrl]-C to abort.
rem echo.
rem
rem goto skip
echo.
echo ==============================
echo Installing Python for Windows
rem echo ------------------------------
rem echo Please choose the following:
rem echo  [x] Install for all users
rem echo  [ ] Skip Test-suite
rem echo  [x] Add python.exe to path
echo ==============================
python-3.11.1-amd64.exe /quiet/passive InstallAllUsers=1 TargetDir=C:\Python311\ PrependPath=1 Include_exe=1 Include_lib=1 Include_launcher=1 InstallLaucherAllUsers=1
set PATH=C:\Python311\;C:\Python311\Scripts;%PATH%
set ERLANG_HOME=%ProgramFiles%\Erlang OTP
echo.
echo ==============================
echo Installing Erlang/OTP
rem echo ------------------------------
rem echo Please choose the following:
rem echo  [ ] Skip Erlang Documentation
echo ==============================
echo.
echo Please wait...
otp_win64_26.2.5.5.exe /S
echo.
netsh advfirewall firewall add rule name=erl.exe dir=in action=allow "program=%PROGRAMFILES%\Erlang OTP\bin\erl.exe" enable=yes
netsh advfirewall firewall add rule name=epmd.exe dir=in action=allow "program=%PROGRAMFILES%\Erlang OTP\erts-14.2.5.4\bin\epmd.exe" enable=yes
netsh advfirewall firewall add rule name=erl2.exe dir=in action=allow "program=%PROGRAMFILES%\Erlang OTP\erts-14.2.5.4\bin\erl.exe" enable=yes
rem pause
:skip
set ERLANG_HOME=%ProgramFiles%\Erlang OTP
echo.
echo ==============================
echo Installing RabbitMQ
echo ==============================
setx /M RABBITMQ_BASE %PROGRAMDATA%\RabbitMQ
set RABBITMQ_BASE=%PROGRAMDATA%\RabbitMQ
setx /M RABBITMQ_NODENAME %COMPUTERNAME%@localhost
set RABBITMQ_NODENAME=%COMPUTERNAME%@localhost
sleep 5
set SYSTEM_COOKIE=C:\WINDOWS\system32\config\systemprofile\.erlang.cookie
set USER_COOKIE=C:\Users\%USERNAME%\.erlang.cookie
if exist %SYSTEM_COOKIE% del /F %SYSTEM_COOKIE%
if exist %USER_COOKIE% del /F %USER_COOKIE%
sleep 5
start /Wait rabbitmq-server-3.13.7.exe
:loop
rem goto weiter
echo Waiting for: %SYSTEM_COOKIE%
if not exist %SYSTEM_COOKIE% sleep 5 & goto :loop
sleep 5
type %USER_COOKIE%
xcopy /Y/H/R %SYSTEM_COOKIE% %USER_COOKIE%
:weiter
echo Cookies:
type %SYSTEM_COOKIE%
echo.
type %USER_COOKIE%
echo.
pause
set RABBITMQ_SBIN=%ProgramFiles%\RabbitMQ Server\rabbitmq_server-3.13.7\sbin
call "%RABBITMQ_SBIN%\rabbitmq-service.bat" remove
copy /Y rabbitmq.config %RABBITMQ_BASE%\rabbitmq.config
call "%RABBITMQ_SBIN%\rabbitmq-service.bat" install
call "%RABBITMQ_SBIN%\rabbitmq-service.bat" start
sleep 10
call "%RABBITMQ_SBIN%\rabbitmq-plugins.bat" enable rabbitmq_management
call "%RABBITMQ_SBIN%\rabbitmq-plugins.bat" enable rabbitmq_shovel
call "%RABBITMQ_SBIN%\rabbitmq-plugins.bat" enable rabbitmq_shovel_management
sleep 5
xcopy /Y /S admin_scripts %RABBITMQ_BASE%\admin_scripts\
cd %RABBITMQ_BASE%\admin_scripts
C:\eurosuite\cygwin64\bin\bash.exe 10.base
:the_end
echo RMQ: Done.

## vars

#!/bin/bash
HOST=localhost
GUEST_USER=guest
GUEST_PW=guest
ADMIN_USER=rmqadmin
ADMIN_PW=nimdaqmr
USER=stduser
PW=stduser
VHOST=eurosuite
EXCHANGE=$DIRECTION"_"$MESSAGETYPE
DEAD_LETTER_EXCHANGE=$DIRECTION"_"$MESSAGETYPE"_deadletter"
DEAD_LETTER_QUEUE=$DIRECTION"_"$MESSAGETYPE"_deadletter"
DEAD_LETTER_ROUTING_KEY="DEADLETTER"
ROOT_PATH="0"
COUNTRY_PATH=$ROOT_PATH"_"$COUNTRY
CHANNEL_PATH=$COUNTRY_PATH"_"$CHANNEL
STORE_PATH=$CHANNEL_PATH"_"$STORE
ENDNODE_PATH=$STORE_PATH"_"$ENDNODE
STORE_QUEUE=$DIRECTION"_"$MESSAGETYPE"_store_"$STORE_PATH
ENDNODE_QUEUE=$DIRECTION"_"$MESSAGETYPE"_endnode_"$STORE_PATH"_"$ENDNODE

function createQueue {
    QUEUE=$1
    echo "Create queue <"$QUEUE">..."
    ARGS="{\"x-dead-letter-exchange\": \""$DEAD_LETTER_EXCHANGE"\", \"x-dead-letter-routing-key\": \""$DEAD_LETTER_ROUTING_KEY"\"}"
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST declare queue name=$QUEUE durable=true "arguments=$ARGS"
}

function bindQueue {
    QUEUE=$1
    ROUTING_KEY=$2
    echo "Bind queue <"$QUEUE"> to <"$EXCHANGE"> with <"$ROUTING_KEY">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST declare binding source=$EXCHANGE destination=$QUEUE destination_type=queue routing_key=$ROUTING_KEY
}

function createAndBindDeadLetterQueue {
    echo "Create queue <"$DEAD_LETTER_QUEUE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST declare queue name=$DEAD_LETTER_QUEUE durable=true
    echo "Bind queue <"$DEAD_LETTER_QUEUE"> to <"$DEAD_LETTER_EXCHANGE"> with routing key <"$DEAD_LETTER_ROUTING_KEY">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST declare binding source=$DEAD_LETTER_EXCHANGE destination=$DEAD_LETTER_QUEUE destination_type=queue routing_key=$DEAD_LETTER_ROUTING_KEY
}

function deleteQueue {
    QUEUE=$1
    echo "Delete queue <"$QUEUE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST delete queue name=$QUEUE
}

function deleteDeadLetterQueue {
    echo "Delete queue <"$DEAD_LETTER_QUEUE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST delete queue name=$DEAD_LETTER_QUEUE
}

function createExchange {
    echo "Create exchange <"$EXCHANGE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST declare exchange name=$EXCHANGE type=topic auto_delete=false internal=false durable=true
    echo "Create exchange <"$DEAD_LETTER_EXCHANGE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST declare exchange name=$DEAD_LETTER_EXCHANGE type=topic auto_delete=false internal=true durable=true
}

function deleteExchange {
    echo "Delete exchange <"$EXCHANGE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST delete exchange name=$EXCHANGE
    echo "Delete exchange <"$DEAD_LETTER_EXCHANGE">..."
    $RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW -V $VHOST delete exchange name=$DEAD_LETTER_EXCHANGE
}

## 10.bash

#!/bin/bash
#set -vx
. ./vars
RABBITMQADMIN="./rabbitmqadmin.py"
# Create administrator account and set permissions for /
$RABBITMQADMIN -u $GUEST_USER -p $GUEST_PW declare user name=$ADMIN_USER password=$ADMIN_PW tags=administrator
$RABBITMQADMIN -u $GUEST_USER -p $GUEST_PW declare permission vhost=/ user=$ADMIN_USER configure=.* write=.* read=.* 
# Delete guest account
$RABBITMQADMIN -u $ADMIN_USER -p $ADMIN_PW delete user name=$GUEST_USER
# Create standard user account
$RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW declare user name=$USER password=$PW tags=monitoring
# Create virtual host
$RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW declare vhost name=$VHOST
# Set administration permissions for admin
$RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW declare permission vhost=$VHOST user=$ADMIN_USER configure=.* write=.* read=.* 
# Set permissions for user
$RABBITMQADMIN -H $HOST -u $ADMIN_USER -p $ADMIN_PW declare permission vhost=$VHOST user=$USER configure= write=.* read=.*