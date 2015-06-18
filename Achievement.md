Jun 18, 2015
---
###benchmark
#####wiki of benchmark
In computing, a benchmark is the act of running a computer program, a set of programs, or other operations, in order to assess the relative performance of an object, normally by running a number of standard tests and trials against it. The term 'benchmark' is also mostly utilized for the purposes of elaborately designed benchmarking programs themselves.

Therefore benchmark includes mainly two part, instance and testing example.

For golang, we only need to implement concrete testing case, and for user code run user code to record this performance.
```
go test -bench=".*" -cpuprofile=cpu.prof -c
```
cpuprofile generate cpu profile, produce binary status program.
it could use go tool pprof to produce specific timing cost status picture
test
```
go tool pprof mymysql.test cpu.prof
```

Jun 16, 2015
---
###contribute format 
The patch’s subject (the commit message’s first line) should:
- begin with an uppercase letter
- be written in the present tense
- not exceed 72 characters in length
- not end with a period
- be prefixed with Fix: if the commit fixes a bug

The commit message’s body should be as detailed as possible and explain the reasons behind the proposed change. Any related bug report(s) should be mentioned at the end of the message using the #123 format, where 123 is the bug number. Use Fixes: #123 to signify that this patch fixes the bug.

Please note that patches should be as focused as possible. Do not, for instance, fix a bug and correct the indentation of an unrelated block of code as part of the same patch.

etcd contribution rules:
https://github.com/coreos/etcd/blob/master/CONTRIBUTING.md

Jun 14, 2015
---
###Batching thinking
when do same thing repeatly, please rethink a batch process method to do it.
For example testing, do redundant check code is embarrassing  
https://github.com/golang/go/wiki/TableDrivenTests

Jun 13, 2015
---
###proxychains and http_proxy
proxychains change the ld method, ld preload the substituted function from static linkage. 
Http_proxy only change the bash ostensible link configuration. 
Becuase golang doesn't use static linkage, "go get " only could link the web through http_proxy config. 
Git uses proxychains to substitute its config

Jun 11, 2015
---
###sudo env

when linux run sudo command, it doesn't inherit general enviorment variable
one solution is user a tricky
add enviroment variables to sudoers config:
```
 sudo visudo
```
add these lines
```
Defaults env_keep += "ftp_proxy http_proxy https_proxy no_proxy...(pointed env variable)"
```

othe solution is 
```
export HTTP_PROXY=foof
sudo -E bash -c 'echo $HTTP_PROXY'
```

Jun 7, 2015
---
###Docker trait
The smallest logic unit is the image.
Container run images.
Hub store images.
Expose container port/mount to user by command

Boot2Docker start a dameo to listen docker command, and emulate a docker server
docker run a contianer stands by serveal images, firstly searching local direcotry, if not exist, it search into Docker hub.

```
boot2docker shellinit
```
a command to export docker env

```
docker run -d -P --name web nginx
```
 The -d flag keeps the container running in the background after the docker run command completes. The -P flag publishes exposed ports from the container to your local host; this lets you access them from your PC.

The -t flag assigns a pseudo-tty or terminal inside our new container and the -i flag allows us to make an interactive connection by grabbing the standard in (STDIN) of the container.

```
docker inspect
docker top
```
check the status of specific container

You also saw how you can bind a container's ports to a specific port using the -p flag:

```
$ sudo docker run -d -p 5000:5000 training/webapp python app.py
```

commit and push commend to extends a image form exist ones to your Docker Hub
```
```
dockerfile is other way to extend image,Each instruction prefixes a statement and is capitalized.
```
INSTRUCTION statement
```
Ex.
```
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```
The first instruction FROM tells Docker what the source of our image is, in this case we're basing our new image on an Ubuntu 14.04 image.

Next we use the MAINTAINER instruction to specify who maintains our new image.

Lastly, we've specified two RUN instructions. A RUN instruction executes a command inside the image, for example installing a package. Here we're updating our APT cache, installing Ruby and RubyGems and then installing the Sinatra gem.

#####check

Jun 3, 2015
---
###exec packages of golang
store the command, exec in bash, cmd.Stdout and cmd.Stderr could access the stdout, stderr.
```
package main

import (
    "log"
    "os/exec"
)

func main() {
    argv := []string{"-port", "10024"}
    cmd := exec.Command("./sample_mapper_user_program/sample_mapper_server", argv...)
    err := cmd.Start()
    if err != nil {
        log.Fatal(err)
    }
    //  var out bytes.Buffer
    // cmd.Stdout = &out
    // w := bytes.NewBuffer(nil)
    // cmd.Stderr = w
    // cmd.Wait()
    // fmt.Printf("in all caps: %q\n", out.String())
    // fmt.Printf("output: %s\n", w.Bytes())
}
```


May 26, 2015
---
##context package of Golang
decript a context in network to check, gain request
solve the request value, deadline, authorization tokens of goroutines to access backends such as databases and RPC services
Backgroud() :
generate a empty context(see details in GoDoc)

WithValue() : 
WithValue returns a copy of parent in which the value associated with key is val.
Use context Values only for request-scoped data that transits processes and APIs, not for passing optional parameters to functions.

For instance :
grpc could use it to check whether meta message is right or not

May 23, 2015
---
##Sass
sass is a framework to implement css easier
gem install sass
sass –watch scss/app.scss:www/css/style.css
watch a scss file changing a css file
details see the official website

May 22, 2015
--------------
##terminal http server
export http_proxy='http://YOUR_USERNAME:YOUR_PASSWORD@PROXY_IP:PROXY_PORT/'
export https_proxy='http://YOUR_USERNAME:YOUR_PASSWORD@PROXY_IP:PROXY_PORT/'

May 19, 2015
--------------
##ps aux 
to view the detailed info of process
##cmd+shift+h in ITerm2
view the history of copy in ITerm 