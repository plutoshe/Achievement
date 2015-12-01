###Apple Input source missing
I got over it. Delete the file "com.apple.HIToolbox.plist", and run sudo rm -f /System/Library/Caches/com.apple.IntlDataCache.le* . Then it worked.

###Godep save failed
Godep need to update, following steps listed below
```
To update a package from your $GOPATH, do this:

Run go get -u foo/bar
Run godep update foo/bar. (You can use the ... wildcard, for example godep update foo/...).
Before committing the change, you'll probably want to inspect the changes to Godeps, for example with git diff, and make sure it looks reasonable.


```
yet when godep update repo, it occurs "no packages can be updated"
```
This seems to be caused by this line here: https://github.com/tools/godep/blob/master/update.go#L205

If packages A and B are under the same root, and I try to only update B, the root will be marked for skipping update because A isn't being updated. I'm not sure what the motivation for this feature is, it seems that developers should be able to selectively update sub packages as they desire.

For what it's worth, I fixed my problem by globbing from the root in my godep update command (e.g. godep update github.com/foo/bar/... instead of github.com/foo/bar/pkg/B. A helpful error message would have gone a long way
```

###golang coverage
```
go test -coverprofile <filename> <package name>
$ go test -coverprofile=cover.out ./util
ok  github.com/mlafeldt/chef-runner/util  0.017s  coverage: 78.1% of statements
$ go tool cover -html=cover.out -o coverage.html
```
###use script to test coverage
write a script to test every package coverage(look for github sample repo)
./script/coverage

Nov 23, 2015
---
###%q,%v format

Nov 23, 2015
---
###golang map delete
```
  delete(map,key)
```
###golang path/filepath

###golang os package permission


###golang time package
time<->Unix.Time
  - Use time.Unix(sec, nanosec) convert unixtime to time.Time
  - Use time_variable.Unix() to get unix time

time<->string
  - have a format to conduct current time variable string 

###Vim turn page

In navigation mode, ctrl-f scrolls down a page and ctrl-b scrolls up a page (think "F"orward and "B"ack). Ctrl-d scrolls down half a page, and ctrl-u scrolls up half a page.


http://superuser.com/questions/335944/vim-in-osx-how-to-do-page-up-page-down-go-to-eol-through-a-vim-file-opened-in-t

###DB should be recorded
RecordIO
SStable
big table
levelDB

###protobuf
Message specify
like json, Marshal and Unmarshal
protobuf print a "\n" in first line, why?


Nov 13, 2015
---
###golang flag array###

One Method uses string, and parses it into our wanted.
Another one uses Value interface to define a value type, supporting flag.Var to use it.
```
type instlice []int
var myints intslice
flag.Var(&myints, "i", "List of integers")
```


```
type Value interface {
    String() string
    Set(string) error
}
```

```
func (i *intslice) String() string {
    return fmt.Sprintf("%d", *i)
}
 
func (i *intslice) Set(value string) error {
    fmt.Printf("%s\n", value)
    tmp, err := strconv.Atoi(value)
    if err != nil {
        *i = append(*i, -1)
    } else {
        *i = append(*i, tmp)
    }
    return nil
}
```
Nov 11, 2015
---
###mongo dump data
./mongodump -d my_mongodb
./mongorestore -d my_mongodb my_mongodb_dump/*
 
###build-web-application-with-golang chapter 3
Let's take a look at the whole execution flow.

Call http.HandleFunc
Call HandleFunc of DefaultServeMux
Call Handle of DefaultServeMux
Add router rules to map[string]muxEntry of DefaultServeMux
Call http.ListenAndServe(":9090", nil)
Instantiate Server
Call ListenAndServe method of Server
Call net.Listen("tcp", addr) to listen to port
Start a loop and accept requests in the loop body
Instantiate a Conn and start a goroutine for every request: go c.serve()
Read request data: w, err := c.readRequest()
Check whether handler is empty or not, if it's empty then use DefaultServeMux
Call ServeHTTP of handler
Execute code in DefaultServeMux in this case
Choose handler by URL and execute code in that handler function: mux.handler.ServeHTTP(w, r)
How to choose handler: A. Check router rules for this URL B. Call ServeHTTP in that handler if there is one C. Call ServeHTTP of NotFoundHandler otherwise

###build-web-application-with-golang chapter 4
Declare how we handler different query/form data(like file/formdata)
Including xss, and using token to prevent duplicate submits.

###json encoding/decoding
Suppose variable v is our objective we want to marshal into json.
General way,
```
  s, err := json.Marshal(v)  
```
In the examples above we always used bytes and strings as intermediates between the data and JSON representation on standard out. We can also stream JSON encodings directly to os.Writers like os.Stdout or even HTTP response bodies.
```
  en := json.NewEncoder(os.Stdout)
  err = en.Encode(v)
```
As is decoding/unmarshal.
reference:
[golang json](https://gobyexample.com/json)

###golang handler test
```
handler := dhttp.NewCounterHandler(m)
  // Do some GET requests with different channel IDs and event filters.
  for i, tt := range tests {
    var url string
    if tt.requestPath == "" {
      url = "http://" + path.Join("example.com", dhttp.CounterPath(tt.channelID)) + tt.params
    } else {
      url = "http://" + path.Join("exmaple.com", tt.requestPath) + tt.params
    }
    w := testutil.Handle(handler, "GET", url, "")

    if w.Code != tt.wcode {
      t.Errorf("#%d: HTTP status code = %d, want = %d", i, w.Code, tt.wcode)
    }

    if string(w.Body.Bytes()) != tt.wbody {
      t.Errorf("#%d: HTTP response body = %q, want = %q", i, string(w.Body.Bytes()), tt.wbody)
    }
  }
```
###golang interface{} to map
use bson.M to achieve some function of map
rc := resCount.(bson.M)

###flagset
```
  fs := flag.NewFlagSet("deepstatsd", flag.ExitOnError)
  mongoDB := fs.String("mongodb", "deepstats", "Specify the Mongo database")
  mongoColl := fs.String("mongocoll", "counter", "Specify the Mongo collection")

  if err := fs.Parse(os.Args[1:]); err != nil {
    log.Println(err)
    os.Exit(1)
  }
```


Nov 10, 2015
---
###check radio box value
```
slice:=[]int{1,2}

for _, v := range slice {
    if v == r.Form.Get("gender") {
        return true
    }
}
return false
```
###sublime shortcut
command + d: Select the current word and the next same word

Nov 9, 2015
---
###GOlang Details
B is a struct
(*B).a is equivalent to B.a

runtime.Gosched() means let the CPU execute other goroutines, and come back at some point. Only a approach to implement by concurrency, but run by one thread.

GOMAXPROCS(n) 

for i := range c will not stop reading data from channel until the channel is closed

Host->cache->DNS->upper DNS


July 23, 2015
---
###Further bash command
htop

dmsg
xargs

```
dirlist=$(find $1 -mindepth 1 -maxdepth 1 -type d)

for dir in $dirlist
do
(
  cd $dir
  ls | xargs -I {} -n 1 echo $dir/{}
)
done;
```
    
    
Seq 6, 2015
---
###golang install
Some commands installing golang into linux system without specific version
```
RUN wget https://storage.googleapis.com/golang/go1.5.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.5.linux-amd64.tar.gz
ENV PATH $PATH:/usr/local/go/bin
ENV GOPATH /go/lib:/go/src/app
```

July 22, 2015
---
###server chinese unicode display "???"
When vim/display/java compiler could not recognize chinese character, it may be the server's locale setting issue.
What finally helped was putting to the file /etc/environment:
LC_ALL=en_US.UTF-8
LANG=en_US.UTF-8
After env setting settled, the issue is fixed.

######locale 
Check the locale language configuration
/etc/default/locale to change the default local configuration

Jun 29, 2015
---
###Latency number of access different component 
Latency Comparison Numbers
--------------------------
L1 cache reference                            0.5 ns
Branch mispredict                             5   ns
L2 cache reference                            7   ns             14x L1 cache
Mutex lock/unlock                            25   ns
Main memory reference                       100   ns             20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy              3,000   ns
Send 1K bytes over 1 Gbps network        10,000   ns    0.01 ms
Read 4K randomly from SSD*              150,000   ns    0.15 ms
Read 1 MB sequentially from memory      250,000   ns    0.25 ms
Round trip within same datacenter       500,000   ns    0.5  ms
Read 1 MB sequentially from SSD*      1,000,000   ns    1    ms  4X memory
Disk seek                            10,000,000   ns   10    ms  20x datacenter roundtrip
Read 1 MB sequentially from disk     20,000,000   ns   20    ms  80x memory, 20X SSD
Send packet CA->Netherlands->CA     150,000,000   ns  150    ms

reference:
https://gist.github.com/jboner/2841832

Jun 25, 2015
---
###dynamic linkers && LD_PRELOAD
invoked by [proxy install in mach](https://github.com/rofl0r/proxychains-ng)
ProxyChains is a UNIX program, that hooks network-related libc functions
  in DYNAMICALLY LINKED programs via a preloaded DLL (dlsym(), LD_PRELOAD)
  and redirects the connections through SOCKS4a/5 or HTTP proxies.
  It supports TCP only (no UDP/ICMP etc).

LD_PRELOAD reference:
http://jvns.ca/blog/2014/11/27/ld-preload-is-super-fun-and-easy/
http://www.catonmat.net/blog/simple-ld-preload-tutorial-part-2/
https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/
https://en.wikipedia.org/wiki/Dynamic_linker


Jun 19, 2015
---
###free port of localhost

Port 0 trick: on both Windows and Linux, if you bind a socket to port 0, the kernel will assign it a free port number somewhere above 1024(A random free port from 1024 to 65535 will be selected.). 

Truly well-written software (e.g. Jetty) will not only let you configure it to bind to port 0, but will make it easy to parse its logs to obtain the actual port number it got assigned.


Jun 18, 2015
---
###benchmark
#####wiki of benchmark
In computing, a benchmark is the act of running a computer program, a set of programs, or other operations, in order to assess the relative performance of an object, normally by running a number of standard tests and trials against it. The term 'benchmark' is also mostly utilized for the purposes of elaborately designed benchmarking programs themselves.
#####realization
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