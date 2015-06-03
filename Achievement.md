June 3, 2015
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