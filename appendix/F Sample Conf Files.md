# Appendix F: Sample Configuration Files and Templates

Hey there! Remember all those cool concepts we dove into back in Chapter 8 about networking and then in Chapter 12 when we started wrangling servers? Yeah, Domain Name System (DNS), Apache, Nginx, Samba, email servers – it was a lot, right? But you made it through! And now? Now, it's time to get seriously hands-on. Theory is awesome, but let's face it, the real magic happens when you’re actually configuring things yourself.

Think of this appendix as your cheat sheet, your quick-start guide to jumpstarting those services we talked about. We're going to look at demo configuration files – not some complex, production-level setups, but clean, understandable examples to get you started. These are the templates you can build on, experiment with, and eventually customize to create your own awesome server configurations.

Think of a configuration file like the instruction manual for a service. It tells the service _exactly_ how to behave, what to do, and how to interact with the world. Mess it up, and the service might not work. Get it right, and boom – you’ve got a web server serving pages, a file server sharing files, or an email server sending messages across the internet! Pretty powerful stuff, huh?

So, let’s dive into some examples and bring those concepts from earlier chapters to life. No more abstract theory – let’s see some actual config files!

Let’s break this down into categories to keep things organized.

## 1) Web Servers: Apache & Nginx – Serving Up the Web

Remember Apache and Nginx from Chapter 12? These are the workhorses of the internet, serving up websites to millions of users every second. While they do the same job, they have different styles. Apache is like the old, reliable craftsman – incredibly versatile and configurable. Nginx is the sleek, modern speedster – known for its efficiency and speed, especially with lots of connections.

Let's look at a basic virtual host configuration for each. Virtual hosts are crucial when you want to host multiple websites on a single server.

### a) Apache – `000-default.conf` (Example Virtual Host)

When you install Apache, you’ll often find a default configuration file, something like `000-default.conf`. Let's see what's inside:

```conf
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName your_domain.com
    ServerAlias www.your_domain.com
    DocumentRoot /var/www/your_domain.com

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/your_domain.com/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

**What’s going on here?**

*   **`<VirtualHost *:80>`:** This starts the virtual host block. `*:80` means this virtual host listens on all IP addresses of the server on port 80 (the standard HTTP port).
*   **`ServerAdmin webmaster@localhost`:** This is the email address of the server administrator. Good practice to set this so people know who to contact if there's a problem.
*   **`ServerName your_domain.com`:** Replace `your_domain.com` with your actual domain name! This tells Apache which domain this configuration is for.
*   **`ServerAlias www.your_domain.com`:** Often, you want your website to work with both `your_domain.com` and `www.your_domain.com`. This alias makes that happen.
*   **`DocumentRoot /var/www/your_domain.com`:** This is super important! It tells Apache where the actual website files (HTML, images, etc.) are located on your server. You’d need to create this directory and put your website files inside it.
*   **`ErrorLog` and `CustomLog`:** These lines define where Apache should store error logs and access logs. Logs are your best friend for troubleshooting and seeing who’s visiting your website.
*   **`<Directory /var/www/your_domain.com/>` block:** This section sets permissions for the `DocumentRoot` directory.
    *   `Options Indexes FollowSymLinks`: Allows directory listing if no index file is found and allows symbolic links to be followed.
    *   `AllowOverride All`: Allows `.htaccess` files (per-directory configuration) to override these settings.
    *   `Require all granted`: Grants access to everyone. For a public website, this is usually what you want.

### b) Nginx – `default` (Example Server Block)

Nginx uses "server blocks" instead of "virtual hosts," but the concept is similar. Here's a basic `default` server block:

    server {
        listen 80;
        server_name your_domain.com www.your_domain.com;
    
        root /var/www/your_domain.com;
        index index.html index.htm index.nginx-debian.html;
    
        location / {
            try_files $uri $uri/ =404;
        }
    
        error_log /var/log/nginx/error.log;
        access_log /var/log/nginx/access.log;
    }
    

**Breaking down the Nginx config:**

*   **`server { ... }`:** This block defines an Nginx server.
*   **`listen 80;`:** Listens on port 80.
*   **`server_name your_domain.com www.your_domain.com;`:** Similar to `ServerName` and `ServerAlias` in Apache, this tells Nginx which domain names this block serves.
*   **`root /var/www/your_domain.com;`:** Like `DocumentRoot` in Apache, this points to the directory with your website files.
*   **`index index.html index.htm index.nginx-debian.html;`:** Defines the index files that Nginx will look for when a user accesses a directory. `index.html` is the most common.
*   **`location / { ... }` block:** This configures how Nginx handles requests for the root path (`/`).
    *   `try_files $uri $uri/ =404;`: This is a clever Nginx trick. It tries to serve the requested file (`$uri`), then tries to serve the index file in the requested directory (`$uri/`), and if neither is found, it returns a 404 error ("Not Found").
*   **`error_log` and `access_log`:** Same as in Apache - where logs are stored.

## 2. File Servers: Samba & NFS – Sharing Files Like Pros

Remember Samba and NFS from Chapter 12? Samba lets you share files with Windows machines, while NFS is more for Linux/Unix environments.

### a) Samba – `smb.conf` (Example Share)

Samba’s main configuration file is `smb.conf`. Let’s create a simple share:

    [global]
        workgroup = WORKGROUP
        server string = Samba Server %v
        netbios name = fileserver
    
        security = user
        map to guest = bad user
    
    [shared_folder]
        path = /srv/samba/shared
        valid users = @smb_users
        read only = no
        browsable = yes
        public = no
    

**Samba config explained:**

*   **`[global]` section:** Global settings for the Samba server.
    *   `workgroup = WORKGROUP`: Sets the workgroup name (often `WORKGROUP` by default, but you can change it).
    *   `server string = Samba Server %v`: A descriptive string for your Samba server. `%v` shows the Samba version.
    *   `netbios name = fileserver`: The name that will appear in network places.
    *   `security = user`: User-level security – users need to authenticate.
    *   `map to guest = bad user`: If a user isn't valid, treat them as a "bad user" (deny access).
*   **`[shared_folder]` section:** Defines a share named "shared\_folder." This name will be visible to users.
    *   `path = /srv/samba/shared`: The actual directory on the server being shared. You'd need to create `/srv/samba/shared`.
    *   `valid users = @smb_users`: Only users in the `smb_users` group can access this share. You’d need to create this group and add users to it in Linux.
    *   `read only = no`: Allows users to write to the share as well as read.
    *   `browsable = yes`: Makes the share visible in network browsing.
    *   `public = no`: Requires authentication to access, despite being browsable.

## b) NFS – `/etc/exports` (Example Export)

NFS configuration is mainly in `/etc/exports`. Let’s share a directory:

    /srv/nfs/shared 192.168.1.0/24(rw,sync,no_subtree_check)
    

**NFS export explained:**

*   `/srv/nfs/shared`: The directory on the server being shared. Again, you’d need to create this directory.
*   `192.168.1.0/24`: The network that is allowed to access this share. `192.168.1.0/24` means any IP address in the range 192.168.1.1 to 192.168.1.254. You'd adjust this to match your network.
*   `(rw,sync,no_subtree_check)`: Export options:
    *   `rw`: Read-write access.
    *   `sync`: Data is written to disk synchronously before the operation is considered complete (safer, but can be slower).
    *   `no_subtree_check`: Relaxed subtree checking (often needed in simpler setups).

## 3. DNS – `named.conf.local` (Example Zone File)

Remember DNS from Chapter 8? Translating domain names to IP addresses? Let's create a simple forward zone file in `named.conf.local` (for BIND9, a common DNS server):

    zone "yourdomain.com" {
        type master;
        file "/etc/bind/db.yourdomain.com";
    };
    

And then the zone file itself, `/etc/bind/db.yourdomain.com`:

    $TTL    86400
    @       IN      SOA     ns1.yourdomain.com. admin.yourdomain.com. (
                                 2024012001 ; Serial
                                 3600       ; Refresh
                                 1800       ; Retry
                                 604800     ; Expire
                                 86400      ; Negative Cache TTL
                             )
    ;
    @       IN      NS      ns1.yourdomain.com.
    ns1     IN      A       192.168.1.10
    www     IN      A       192.168.1.10
    mail    IN      A       192.168.1.10
    

**DNS Zone File Breakdown:**

*   **`zone "yourdomain.com" { ... };`:** Defines a DNS zone for `yourdomain.com`.
    *   `type master;`: This server is the primary DNS server for this zone.
    *   `file "/etc/bind/db.yourdomain.com";`: Specifies the path to the actual zone data file.
*   **`$TTL 86400`:** Time-to-live for records in this zone (86400 seconds = 1 day).
*   **`SOA` Record:** Start of Authority record – contains crucial zone information.
    *   `ns1.yourdomain.com.`: Primary nameserver for the zone.
    *   `admin.yourdomain.com.`: Administrator's email address (use a dot instead of `@`).
    *   Serial, Refresh, Retry, Expire, Negative Cache TTL – control how DNS records are updated and cached.
*   **`NS` Record:** Name Server record – delegates authority for `yourdomain.com` to `ns1.yourdomain.com`.
*   **`A` Records:** Address records – map hostnames to IP addresses.
    *   `ns1 IN A 192.168.1.10`: Sets the IP address for `ns1.yourdomain.com` to `192.168.1.10`.
    *   `www IN A 192.168.1.10`: Sets the IP address for `www.yourdomain.com` (your website) to `192.168.1.10`.
    *   `mail IN A 192.168.1.10`: Sets the IP address for `mail.yourdomain.com` (your mail server) to `192.168.1.10`.

## 4) FTP – `vsftpd.conf` (Basic Setup)

FTP (File Transfer Protocol) is for transferring files. `vsftpd` is a popular FTP server. Here’s a very basic `vsftpd.conf`:

    listen=YES
    anonymous_enable=NO
    local_enable=YES
    write_enable=YES
    local_umask=022
    dirmessage_enable=YES
    xferlog_enable=YES
    connect_from_port_20=YES
    xferlog_std_format=YES
    chroot_local_user=YES
    allow_writeable_chroot=YES
    pam_service_name=vsftpd
    rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
    rsa_private_key=/etc/ssl/private/ssl-cert-snakeoil.key
    

**Basic `vsftpd.conf` explained:**

*   `listen=YES`: Enables FTP listening.
*   `anonymous_enable=NO`: Disables anonymous FTP logins (more secure).
*   `local_enable=YES`: Allows local Linux users to log in with their system usernames and passwords.
*   `write_enable=YES`: Allows write operations (uploading files).
*   `local_umask=022`: Sets the default file permissions for uploaded files.
*   `chroot_local_user=YES`: Locks users into their home directories for security. `allow_writeable_chroot=YES` is often needed if you want to allow writes inside the chroot (be careful with this in production).

## 5. Email Service – (Simplified Concept)

Configuring a full email server is complex and could be a whole book in itself! But let’s touch on the _idea_ of configuration for email, focusing on a crucial configuration file for Postfix, a common Mail Transfer Agent (MTA). For Postfix, the main config file is `main.cf`. Here’s a very simplified example of some key settings:

    inet_interfaces = all
    mydomain = yourdomain.com
    myhostname = mail.yourdomain.com
    myorigin = /etc/mailname
    mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
    relayhost =
    mynetworks = 127.0.0.0/8, [::ffff:127.0.0.0]/104, [::1]/128, 192.168.1.0/24
    mailbox_size_limit = 0
    recipient_delimiter = +
    inet_protocols = all
    

**Simplified Email Config Concepts (Postfix `main.cf`):**

*   `inet_interfaces = all`: Listen on all network interfaces.
*   `mydomain = yourdomain.com`: Your domain name.
*   `myhostname = mail.yourdomain.com`: The hostname of your mail server.
*   `myorigin = /etc/mailname`: Sets the origin for outgoing mail (often based on `/etc/mailname`).
*   `mydestination = ...`: Domains for which this server is the final destination (local delivery).
*   `relayhost =`: If you want to relay email through another server (like your ISP's), you'd configure it here. Leaving it blank means you're sending emails directly.
*   `mynetworks = ...`: Trusted networks that can relay through this server. Includes localhost and your local network (192.168.1.0/24).

> [!NOTE]
> Setting up a functional email server, especially one that reliably sends and receives emails and avoids being flagged as spam, involves much more, including DNS records (MX, SPF, DKIM, DMARC – as discussed in Chapter 8!), proper reverse DNS, and potentially using services like DKIM and SPF to authenticate your emails. The above is a _very_ basic glimpse to show config file principles.

## Wrapping Up

So, there you have it – a quick tour through some essential configuration files. Remember, these are just starting points! The real learning happens when you start tweaking these configurations, reading the service documentation (man pages are your friend in Linux!), and experimenting. Don't be afraid to break things – that’s often the best way to learn. And always, _always_ back up your configuration files before you make changes!

Think of these config files as recipes. You’ve got the basic ingredients and instructions here. Now, it’s your turn to get in the kitchen and start cooking up your own server configurations. Have fun, and keep exploring! You’ve got this!