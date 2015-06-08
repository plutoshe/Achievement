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