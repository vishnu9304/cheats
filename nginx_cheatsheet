
Notes:

    The basic nginx architecture consists of a master process and its workers.
    Developed to solve c10k problems. Which means handling 10,000 concurrent connections.

    Applications:
        High Performance Web Server
        Reverse Proxy (SSL Termination and Contnet Caching and Termination)
        Load Balancer

    The master is supposed to read the configuration file and maintain worker processes, while workers do the actual processing of requests.

References:
    
    To get info about core context and directive blocks, go to http://nginx.org/en/docs/ngx_core_module.html

Commands:

    While your nginx instance is running, you can manage it by sending signals:

    sudo nginx -s <signal>

    stop: fast shutdown
    quit: graceful shutdown (wait for workers to finish their processes)
    reload: reload the configuration file
    reopen: reopen the log files

    To verify configuration: nginx -t
    To view selinux context: semanage fcontext -l | grep -i /usr/share/nginx/html
    To add selinux context: semanage fcontext -a -t httpd_sys_content_t '/var/www'
    To restore context back to default: restorecon -R -v '/var/www'
    Curl with host header: curl --header "Host: www.example.com" localhost

Configuration files location:

    /etc/nginx/nginx.conf
    /usr/local/etc/nginx/nginx.conf
    /usr/local/nginx/conf/nginx.conf

Directive and Context:

    Directive: The option that consists of name and parameters; it should end with a semicolon

    Example: gzip on;

    Context: Section where you can declare directives (similar to scope in programming languages)

    Example:

    http {              # http context
        gzip on;        # directive in http context
    }

Directive types:

    You have to pay attention when using the same directive in multiple contexts, as the inheritance model differs for different directives. 

    There are 3 types of directives, each with its own inheritance model.

Normal:

    Has one value per context.
    Also, it can be defined only once in the context.
    Subcontexts can override the parent directive, but this override will be valid only in a given subcontext.

        gzip on;
        gzip off;   # illegal to have 2 normal directives in same context

        server {                            # server context
            location /downloads {           # location subcontext under server context

            }
            location /assets {

            }
        }

Array:

    Adding multiple directives in the same context will add to the values instead of overwriting them altogether.
    Defining a directive in a subcontext will override ALL parent values in the given subcontext.

    error_log /var/log/nginx/error.log;
    error_log /var/log/nginx/error_native.log notice;
    error_log /var/log/nginx/error_debug.log debug;

    server {
            location /downloads {
            # this will override all the parent directives
            error_log /var/log/nginx/error_downloads.log;
        }
    }

Action:

    Actions are directives that change things. Their inheritance behaviour will depend on the module.
    For example, in the case of the rewrite directive, every matching directive will be executed:

    server {
        rewrite ^ /foobar;

        location /foobar {
            rewrite ^ /foo;
            rewrite ^ /bar;
        }
    }

Custom Error Pages:

    Syntax: error_page <list of error codes> <error page>
    Example: error_page 404 /404.html

Basic Auth:

    https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html
    location /admin.html {
    	auth_basic "Login Required";
	auth_basic_user_file /etc/nginx/.htpasswd;
    }

nginx SSL: https://nginx.org/en/docs/http/ngx_http_ssl_module.html

Self Singed Certs:

    mkdir /etc/nginx/ssl
    openssl -req x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/keys/private.key -out /etc/nginx/ssl/public.pem

    req - We’re making a certificate request to OpenSSL
    -x509 - Specifying the structure that our certificate should have. Conforms to the X.509 standard
    -nodes - Do not encrypt the output key
    -days 365 - Set the key to be valid for 365 days
    -newkey rsa:2048 - Generate an RSA key that is 2048 bits in size
    -keyout /etc/nginx/ssl/private.key - File to write the private key to
    -out /etc/nginx/ssl/public.pem - Output file for public portion of key

    server {
    listen 443 ssl;
    root /usr/share/nginx/html;
    ssl_certificate /etc/nginx/ssl/home.v01.openhouse.cert.pem;
    ssl_certificate_key /etc/nginx/ssl/keys/home.v01.openhouse.key.pem;
    server_name _;
    location = /admin.html {
    	auth_basic "Login Required";
	auth_basic_user_file /etc/nginx/.htpasswd;
    }
    error_page 404 /404.html;
    }


