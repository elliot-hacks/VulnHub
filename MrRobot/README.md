# Mr.Robot Walkthrough
### From network scans with fping or netdiscover,
### Identify the IP Address of the machine

***
    ┌──(kali㉿kali)-[~]
    └─$ fping -aqg 10.0.2.0/24
    10.0.2.1
    10.0.2.2
    10.0.2.3
    10.0.2.48
    10.0.2.61
 
***


### The machines IP is 10.0.2.48
### Then, it's services with nmap.
### I'm using db_nmap of metasploit

***
    msf6 > db_nmap -p- -A -O -sV -sC 10.0.2.48
        [*] Nmap: Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-05 23:10 EDT
        [*] Nmap: Nmap scan report for 10.0.2.48
        [*] Nmap: Host is up (0.00085s latency).
        [*] Nmap: Not shown: 65532 filtered tcp ports (no-response)
        [*] Nmap: PORT    STATE  SERVICE  VERSION
        [*] Nmap: 22/tcp  closed ssh
        [*] Nmap: 80/tcp  open   http     Apache httpd
        [*] Nmap: |_http-title: Site doesn't have a title (text/html).
        [*] Nmap: |_http-server-header: Apache
        [*] Nmap: 443/tcp open   ssl/http Apache httpd
        [*] Nmap: |_http-title: Site doesn't have a title (text/html).
        [*] Nmap: |_http-server-header: Apache
        [*] Nmap: | ssl-cert: Subject: commonName=www.example.com
        [*] Nmap: | Not valid before: 2015-09-16T10:45:03
        [*] Nmap: |_Not valid after:  2025-09-13T10:45:03
        [*] Nmap: MAC Address: 08:00:27:10:0F:D0 (Oracle VirtualBox virtual NIC)
        [*] Nmap: Aggressive OS guesses: Linux 3.10 - 4.11 (98%), Linux 3.2 - 4.9 (94%), Linux 3.2 - 3.8 (93%), Linux 3.18 (93%), Linux 3.13 (92%), Linux 3.13 or 4.2 (92%), Linux 4.2 (92%), Linux 4.4 (92%), Linux 3.16 (91%), Linux 2.6.26 - 2.6.35 (91%)
        [*] Nmap: No exact OS matches for host (test conditions non-ideal).
        [*] Nmap: Network Distance: 1 hop
        [*] Nmap: TRACEROUTE
        [*] Nmap: HOP RTT     ADDRESS
        [*] Nmap: 1   0.85 ms 10.0.2.48
        [*] Nmap: OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
        [*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 145.48 seconds
    msf6 > services
        Services
        ========

        host       port  proto  name      state   info
        ----       ----  -----  ----      -----   ----
        10.0.2.48  22    tcp    ssh       closed
        10.0.2.48  80    tcp    http      open    Apache httpd
        10.0.2.48  443   tcp    ssl/http  open    Apache httpd



***


### We can try to curl to view anything interesting on the webserver

***
    ┌──(kali㉿kali)-[~]
    └─$ curl http://10.0.2.48
        <!doctype html>
        <!--
        \   //~~\ |   |    /\  |~~\|~~  |\  | /~~\~~|~~    /\  |  /~~\ |\  ||~~
         \ /|    ||   |   /__\ |__/|--  | \ ||    | |     /__\ | |    || \ ||--
          |  \__/  \_/   /    \|  \|__  |  \| \__/  |    /    \|__\__/ |  \||__
        -->
        <html class="no-js" lang="">
        <head>
            

            <link rel="stylesheet" href="css/main-600a9791.css">

            <script src="js/vendor/vendor-48ca455c.js"></script>

            <script>var USER_IP='208.185.115.6';var BASE_URL='index.html';var RETURN_URL='index.html';var REDIRECT=false;window.log=function(){log.history=log.history||[];log.history.push(arguments);if(this.console){console.log(Array.prototype.slice.call(arguments));}};</script>

        </head>
        <body>
            <!--[if lt IE 9]>
            <p class="browserupgrade">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
            

            <!-- Google Plus confirmation -->
            <div id="app"></div>

            
            <script src="js/s_code.js"></script>
            <script src="js/main-acba06a5.js"></script>
        </body>
        </html>
 


***


### Then checked out directories using dirb

***
                                                                                                                                                            
    ┌──(kali㉿kali)-[~]
    └─$ dirb http://10.0.2.48

        -----------------
        DIRB v2.22    
        By The Dark Raver
        -----------------

        START_TIME: Fri Apr  5 23:11:25 2024
        URL_BASE: http://10.0.2.48/
        WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

        -----------------

        GENERATED WORDS: 4612                                                          

        ---- Scanning URL: http://10.0.2.48/ ----
        ==> DIRECTORY: http://10.0.2.48/0/                                                                                                                         
        ==> DIRECTORY: http://10.0.2.48/admin/                                                                                                                     
        + http://10.0.2.48/atom (CODE:301|SIZE:0)                                                                                                                  
        ==> DIRECTORY: http://10.0.2.48/audio/                                                                                                                     
        ==> DIRECTORY: http://10.0.2.48/blog/                                                                                                                      
        ==> DIRECTORY: http://10.0.2.48/css/                                                                                                                       
        + http://10.0.2.48/dashboard (CODE:302|SIZE:0)                                                                                                             
        + http://10.0.2.48/favicon.ico (CODE:200|SIZE:0)                                                                                                           
        ==> DIRECTORY: http://10.0.2.48/feed/                                                                                                                      
        ==> DIRECTORY: http://10.0.2.48/image/                                                                                                                     
        ==> DIRECTORY: http://10.0.2.48/Image/                                                                                                                     
        ==> DIRECTORY: http://10.0.2.48/images/                                                                                                                    
        + http://10.0.2.48/index.html (CODE:200|SIZE:1188)                                                                                                         
        + http://10.0.2.48/index.php (CODE:301|SIZE:0)                                                                                                             
        + http://10.0.2.48/intro (CODE:200|SIZE:516314)                                                                                                            
        ==> DIRECTORY: http://10.0.2.48/js/                                                                                                                        
        + http://10.0.2.48/license (CODE:200|SIZE:309)                                                                                                             
        + http://10.0.2.48/login (CODE:302|SIZE:0)                                                                                                                 
        + http://10.0.2.48/page1 (CODE:301|SIZE:0)                                                                                                                 
        + http://10.0.2.48/phpmyadmin (CODE:403|SIZE:94)                                                                                                           
        + http://10.0.2.48/rdf (CODE:301|SIZE:0)                                                                                                                   
        + http://10.0.2.48/readme (CODE:200|SIZE:64)                                                                                                               
        + http://10.0.2.48/robots (CODE:200|SIZE:41)                                                                                                               
        + http://10.0.2.48/robots.txt (CODE:200|SIZE:41)                                                                                                           
        + http://10.0.2.48/rss (CODE:301|SIZE:0)                                                                                                                   
        + http://10.0.2.48/rss2 (CODE:301|SIZE:0)                                                                                                                  
        + http://10.0.2.48/sitemap (CODE:200|SIZE:0)                                                                                                               
        + http://10.0.2.48/sitemap.xml (CODE:200|SIZE:0)                                                                                                           
        ==> DIRECTORY: http://10.0.2.48/video/                                                                                                                     
        ==> DIRECTORY: http://10.0.2.48/wp-admin/                                                                                                                  
        + http://10.0.2.48/wp-config (CODE:200|SIZE:0)                                                                                                             
        ==> DIRECTORY: http://10.0.2.48/wp-content/                                                                                                                
        + http://10.0.2.48/wp-cron (CODE:200|SIZE:0)                                                                                                               
        ==> DIRECTORY: http://10.0.2.48/wp-includes/                                                                                                               
        + http://10.0.2.48/wp-links-opml (CODE:200|SIZE:227)                                                                                                       
        + http://10.0.2.48/wp-load (CODE:200|SIZE:0)                                                                                                               
        + http://10.0.2.48/wp-login (CODE:200|SIZE:2585)                                                                                                           
        + http://10.0.2.48/wp-mail (CODE:500|SIZE:3025)                                                                                                            
        + http://10.0.2.48/wp-settings (CODE:500|SIZE:0)                                                                                                           
        + http://10.0.2.48/wp-signup (CODE:302|SIZE:0)                                                                                                             
        + http://10.0.2.48/xmlrpc (CODE:405|SIZE:42)                                                                                                               
        + http://10.0.2.48/xmlrpc.php (CODE:405|SIZE:42)                                                                                                           
        

***


### From dirb, there is an interesting file called robots.txt
### If I perform curl on it.

***
    ┌──(kali㉿kali)-[~]
    └─$ curl http://10.0.2.48/robots.txt
        User-agent: *
        fsocity.dic
        key-1-of-3.txt


***


### There is the first flag,
### Together with a dictionary file

***
┌──(kali㉿kali)-[~]
└─$ wget http://10.0.2.48/fsocity.dic    
--2024-04-05 23:44:00--  http://10.0.2.48/fsocity.dic
Connecting to 10.0.2.48:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7245381 (6.9M) [text/x-c]
Saving to: ‘fsocity.dic’

fsocity.dic                            100%[============================================================================>]   6.91M  19.5MB/s    in 0.4s    

2024-04-05 23:44:00 (19.5 MB/s) - ‘fsocity.dic’ saved [7245381/7245381]

                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ wc -l fsocity.dic                
858160 fsocity.dic
                                                                                                                                                            
                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ curl http://10.0.2.48/key-1-of-3.txt
073403c8a58a1f80d943455fb30724b9


***

### There's our first flag with a dictionary of 858160 words
### From dirb, checking urls with response 200, we find wp-login

***
    ┌──(kali㉿kali)-[~]
    └─$ curl http://10.0.2.48/wp-login      
        <!DOCTYPE html>
                <!--[if IE 8]>
                        <html xmlns="http://www.w3.org/1999/xhtml" class="ie8" lang="en-US">
                <![endif]-->
                <!--[if !(IE 8) ]><!-->
                        <html xmlns="http://www.w3.org/1999/xhtml" lang="en-US">
                <!--<![endif]-->
                <head>
                <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
                <title>user&#039;s Blog! &rsaquo; Log In</title>
                <link rel='stylesheet' id='buttons-css' href='http://10.0.2.48/wp-includes/css/buttons.min.css?ver=4.3.1' type='text/css' media='all'/>
        <link rel='stylesheet' id='open-sans-css' href='https://fonts.googleapis.com/css?family=Open+Sans%3A300italic%2C400italic%2C600italic%2C300%2C400%2C600&#038;subset=latin%2Clatin-ext&#038;ver=4.3.1' type='text/css' media='all'/>
        <link rel='stylesheet' id='dashicons-css' href='http://10.0.2.48/wp-includes/css/dashicons.min.css?ver=4.3.1' type='text/css' media='all'/>
        <link rel='stylesheet' id='login-css' href='http://10.0.2.48/wp-admin/css/login.min.css?ver=4.3.1' type='text/css' media='all'/>
        <meta name='robots' content='noindex,follow'/>
                </head>
                <body class="login login-action-login wp-core-ui  locale-en-us">
                <div id="login">
                        <h1><a href="https://wordpress.org/" title="Powered by WordPress" tabindex="-1">user&#039;s Blog!</a></h1>

        <form name="loginform" id="loginform" action="http://10.0.2.48/wp-login.php" method="post">
                <p>
                        <label for="user_login">Username<br/>
                        <input type="text" name="log" id="user_login" class="input" value="" size="20"/></label>
                </p>
                <p>
                        <label for="user_pass">Password<br/>
                        <input type="password" name="pwd" id="user_pass" class="input" value="" size="20"/></label>
                </p>
                        <p class="forgetmenot"><label for="rememberme"><input name="rememberme" type="checkbox" id="rememberme" value="forever"/> Remember Me</label></p>
                <p class="submit">
                        <input type="submit" name="wp-submit" id="wp-submit" class="button button-primary button-large" value="Log In"/>
                        <input type="hidden" name="redirect_to" value="http://10.0.2.48/wp-admin/"/>
                        <input type="hidden" name="testcookie" value="1"/>
                </p>
        </form>

        <p id="nav">
                <a href="http://10.0.2.48/wp-login.php?action=lostpassword" title="Password Lost and Found">Lost your password?</a>
        </p>

        <script type="text/javascript">function wp_attempt_focus(){setTimeout(function(){try{d=document.getElementById('user_login');d.focus();d.select();}catch(e){}},200);}
        wp_attempt_focus();if(typeof wpOnload=='function')wpOnload();</script>

                <p id="backtoblog"><a href="http://10.0.2.48/" title="Are you lost?">&larr; Back to user&#039;s Blog!</a></p>

                </div>


                        <div class="clear"></div>
                </body>
                </html>



***


