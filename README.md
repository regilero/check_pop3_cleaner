check_pop3_cleaner
==================

Nagios python check, connects a POP3 account, check sizes and can delete messages

Always check github for last version: https://github.com/regilero/check_pop3_cleaner

Author: regilero (https://github.com/regilero)

Features
---------

  * Can check both number of messages and total size of mailbox thresolds
  * Can avoid theses checks (``-w , -c , `` syntax)
  * Can delete a given number of messages at each check
  * Debug mode available

This script can be used in conjonction with an smtp script. Send some smtp messages on the first command and then use this script on another command to delete all messages received on a test account. Check the Command examples at the end of this document.

Usage
-----

<pre>
./check_pop3_cleaner.py -h
usage: check_pop3_cleaner [-h] [-v] -H [HOSTNAME] -u [USER] -p [PASSWORD]
                          [-d [DELETE]] [-s] [-a] [-P [PORT]] [-t [TIMEOUT]]
                          [-D] [-w [WARNING]] [-c [CRITICAL]]

Connect a POP3 account, check mailbox size, number of messages and may also
delete some of theses messages.

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program version
  -H [HOSTNAME], --hostname [HOSTNAME]
                        hostname (directory name for burp) [default: None]
  -u [USER], --user [USER]
                        User account to log in 
  -p [PASSWORD], --password [PASSWORD]
                        User account password
  -d [DELETE], --delete [DELETE]
                        How many messages should we delete?[default: 0].
                        Useful for test accounts
  -s, --ssl             If true connect using SSL/TLS
  -a, --apop            If true connect using APOP protocol
  -P [PORT], --port [PORT]
                        POP3 port [default: 110] use 995 for ssl
  -t [TIMEOUT], --timeout [TIMEOUT]
                        Timeout [default: 15]
  -D, --debug           Debug Mode
  -w [WARNING], --warning [WARNING]
                        Warning thresolds pair,
                          * first value is maximum size of mbox in octets,
                          * second value is maximum number of messages
                            in this mbox.
                        Use empty values around the coma to remove a check
                        for size or number [default: 1048576,200]
  -c [CRITICAL], --critical [CRITICAL]
                        Critical thresolds pair,
                          * first value is maximum size of mbox in octets,
                          * second value is maximum number of messages
                            in this mbox.
                        Use empty values around the coma to remove a check
                        for size or number [default: 10485760,1000]

Note that if you activate the delete part the number of messages are taken
before the delete operation.
</pre>


Examples
--------

    ~# ./check_pop3_cleaner.py  -H imap.example.com -D -u foo@example.com 
      -p "iskJHjsyHNpopmlkT2IKUDj" -t 10 --delete=10 -w 524288,
       -c 1048576,2000
    ~# ./check_pop3_cleaner.py  -H imap.example.com -D -u bar 
      -p "ams25IUqsnBD25dd4" -d 25 -t 10 -w 262144,50 -c 524288,100
    ~# ./check_pop3_cleaner.py  -H imap.example.com -D -u bar 
      -p "ams25IUqsnBD25dd4" -d 25 -t 10 -w , -c ,

Output Samples
--------------

    POP3 WARNING - Size of mailbox is high: 1.9 MB>=1.0 MB and number of messages is high also: 3237>=100 | size=2014159;1048576;3048576;;3048576; messages=3237;100;20000;;20000;
    
    POP3 CRITICAL - Size of mailbox is too high: 1.9 MB>=1.0 MB | size=1998565;548576;1048576;;1048576; messages=3212;100;20000;;20000;

    

Command example
---------------

