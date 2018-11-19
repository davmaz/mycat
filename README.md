On linux, install docker: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
sudo usermod -aG docker ${USER}

To apply the new group membership, log out of the server and back in, or type the following:

su - ${USER}
You will be prompted to enter your user's password to continue.

Confirm that your user is now added to the docker group by typing:

id -nG

(the above is important -- Duh)!!

docker
should show the help screen

Now
docker run --rm -it golang:11.1

go env GOPATH
will show /go as the GOPATH
(Recap:
 go version
 cd
 go env GOPATH
 mkdir mycat
 cd mycat
 go env GOPATH
)

But we're going to develop OUTSIDE gopath
cd ..
mkdir mycat
cd mycat
pwd (should show .../mycat -- basically you're under the root directory, then mycat)
Now install vim in the container:
apt-get update && apt-get install -y vim
(docker downloading and installing vim...)
root@58be8a1015a8:~/mycat# vim main.go
root@58be8a1015a8:~/mycat# cat main.go
package main

import  (
	
)

func main() {
	_, err := io.Copy(os.Stdout, os.Stdin)
	if err != nil {
		log.Fatal(err)
	}
}
root@58be8a1015a8:~/mycat# vi main.go
root@58be8a1015a8:~/mycat# gofmt -w main.go
root@58be8a1015a8:~/mycat# go build
root@58be8a1015a8:~/mycat# ls
main.go  mycat
root@58be8a1015a8:~/mycat# ./mycat
test
test
root@58be8a1015a8:~/mycat# rm -f mycat
root@58be8a1015a8:~/mycat# ls
main.go
root@58be8a1015a8:~/mycat# vim main.go
root@58be8a1015a8:~/mycat# go fmt main.go
main.go
root@58be8a1015a8:~/mycat# cat main.go
package main

import (
	"github.com/sirupsen/logrus"
	"io"
	"os"
)

func main() {
	_, err := io.Copy(os.Stdout, os.Stdin)
	if err != nil {
		logrus.Fatal(err)
	}
}
root@58be8a1015a8:~/mycat# go run main.go
main.go:4:2: cannot find package "github.com/sirupsen/logrus" in any of:
	/usr/local/go/src/github.com/sirupsen/logrus (from $GOROOT)
	/go/src/github.com/sirupsen/logrus (from $GOPATH)
root@58be8a1015a8:~/mycat# go get
go get: no install location for directory /root/mycat outside GOPATH
	For more details see: 'go help gopath'
root@58be8a1015a8:~/mycat# go mod init
go: cannot determine module path for source directory /root/mycat (outside GOPATH, no import comments)
root@58be8a1015a8:~/mycat# go mod init gihub.com/davmaz/mycat
go: creating new go.mod: module gihub.com/davmaz/mycat
root@58be8a1015a8:~/mycat# ls
go.mod	main.go
root@58be8a1015a8:~/mycat# cat go.mod
module gihub.com/davmaz/mycat
root@58be8a1015a8:~/mycat# ls
go.mod	main.go
root@58be8a1015a8:~/mycat# go build
go: finding github.com/sirupsen/logrus v1.2.0
go: downloading github.com/sirupsen/logrus v1.2.0
go: finding github.com/stretchr/objx v0.1.1
go: finding github.com/konsorten/go-windows-terminal-sequences v1.0.1
go: finding github.com/stretchr/testify v1.2.2
go: finding github.com/pmezard/go-difflib v1.0.0
go: finding github.com/davecgh/go-spew v1.1.1
go: finding golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33
go: finding golang.org/x/crypto v0.0.0-20180904163835-0709b304e793
go: downloading golang.org/x/crypto v0.0.0-20180904163835-0709b304e793
go: downloading golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33

Now, with just go mod init it can find the import it needs

root@58be8a1015a8:~/mycat# cat go.mod
module gihub.com/davmaz/mycat

require github.com/sirupsen/logrus v1.2.0

Notice is has the import for logrus

root@58be8a1015a8:~/mycat# cat go.sum
github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/konsorten/go-windows-terminal-sequences v1.0.1/go.mod h1:T0+1ngSBFLxvqU3pZ+m/2kptfBszLMUkC4ZK/EgS/cQ=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/sirupsen/logrus v1.2.0 h1:juTguoYk5qI21pwyTXY3B3Y5cOTH3ZUyZCg1v/mihuo=
github.com/sirupsen/logrus v1.2.0/go.mod h1:LxeOpSwHxABJmUn/MG1IvRgCAasNZTLOkJPxbbu5VWo=
github.com/stretchr/objx v0.1.1/go.mod h1:HFkY916IF+rwdDfMAkV7OtwuqBVzrE8GR6GFx+wExME=
github.com/stretchr/testify v1.2.2/go.mod h1:a8OnRcib4nhh0OaRAV+Yts87kKdq0PP7pXfy6kDkUVs=
golang.org/x/crypto v0.0.0-20180904163835-0709b304e793 h1:u+LnwYTOOW7Ukr/fppxEb1Nwz0AtPflrblfvUudpo+I=
golang.org/x/crypto v0.0.0-20180904163835-0709b304e793/go.mod h1:6SG95UA2DQfeDnfUPMdvaQW0Q7yPrPDi9nlGo2tz2b4=
golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33 h1:I6FyU15t786LL7oL/hn43zqTuEGr4PN7F4XJ1p4E3Y8=
golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=

This is amazing -- all the dependencies for mycat.go are contained here.
Notice the cryptographic HASH for the packages underlined above so we will know it's unique.
So if you want to use v1.1 just change the go.mod value
root@58be8a1015a8:~/mycat# vi go.mod
root@58be8a1015a8:~/mycat# go build
go: finding github.com/sirupsen/logrus v1.1.0
go: finding github.com/konsorten/go-windows-terminal-sequences v0.0.0-20180402223658-b729f2633dfe
go: downloading github.com/sirupsen/logrus v1.1.0

root@58be8a1015a8:~/mycat# cat go.sum
github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/konsorten/go-windows-terminal-sequences v0.0.0-20180402223658-b729f2633dfe/go.mod h1:T0+1ngSBFLxvqU3pZ+m/2kptfBszLMUkC4ZK/EgS/cQ=
github.com/konsorten/go-windows-terminal-sequences v1.0.1/go.mod h1:T0+1ngSBFLxvqU3pZ+m/2kptfBszLMUkC4ZK/EgS/cQ=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/sirupsen/logrus v1.1.0 h1:65VZabgUiV9ktjGM5nTq0+YurgTyX+YI2lSSfDjI+qU=
github.com/sirupsen/logrus v1.1.0/go.mod h1:zrgwTnHtNr00buQ1vSptGe8m1f/BbgsPukg8qsT7A+A=
github.com/sirupsen/logrus v1.2.0 h1:juTguoYk5qI21pwyTXY3B3Y5cOTH3ZUyZCg1v/mihuo=
github.com/sirupsen/logrus v1.2.0/go.mod h1:LxeOpSwHxABJmUn/MG1IvRgCAasNZTLOkJPxbbu5VWo=
github.com/stretchr/objx v0.1.1/go.mod h1:HFkY916IF+rwdDfMAkV7OtwuqBVzrE8GR6GFx+wExME=
github.com/stretchr/testify v1.2.2/go.mod h1:a8OnRcib4nhh0OaRAV+Yts87kKdq0PP7pXfy6kDkUVs=
golang.org/x/crypto v0.0.0-20180904163835-0709b304e793 h1:u+LnwYTOOW7Ukr/fppxEb1Nwz0AtPflrblfvUudpo+I=
golang.org/x/crypto v0.0.0-20180904163835-0709b304e793/go.mod h1:6SG95UA2DQfeDnfUPMdvaQW0Q7yPrPDi9nlGo2tz2b4=
golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33 h1:I6FyU15t786LL7oL/hn43zqTuEGr4PN7F4XJ1p4E3Y8=
golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=

Both of the go.mod and go.sum should be put on github, also
the go.mod will update accordingly if you decide to go back to the previous version:

root@58be8a1015a8:~/mycat# go get -u github.com/sirupsen/logrus
go: finding golang.org/x/crypto latest
go: finding golang.org/x/sys latest
go: downloading golang.org/x/crypto v0.0.0-20181112202954-3d3f9f413869
go: downloading golang.org/x/sys v0.0.0-20181116161606-93218def8b18
root@58be8a1015a8:~/mycat# 
root@58be8a1015a8:~/mycat# cat go.mod
module gihub.com/davmaz/mycat

require (
	github.com/sirupsen/logrus v1.2.0
	golang.org/x/crypto v0.0.0-20181112202954-3d3f9f413869 // indirect
	golang.org/x/sys v0.0.0-20181116161606-93218def8b18 // indirect
)

Now, this is really cool: go mod tidy

root@58be8a1015a8:~/mycat# go mod tidy
go: downloading github.com/konsorten/go-windows-terminal-sequences v1.0.1
go: downloading github.com/stretchr/testify v1.2.2
go: downloading github.com/davecgh/go-spew v1.1.1
go: downloading github.com/pmezard/go-difflib v1.0.0

Now it will include the test packages.
Now we have to use the vendor command so it will get all the dependencies
They are all stored under this GOPATH package's pkg directory
root@58be8a1015a8:~/mycat# find /go/pkg/mod/
/go/pkg/mod/
/go/pkg/mod/github.com
/go/pkg/mod/github.com/sirupsen
/go/pkg/mod/github.com/sirupsen/logrus@v1.2.0
/go/pkg/mod/github.com/sirupsen/logrus@v1.2.0/example_global_hook_test.go
/go/pkg/mod/github.com/sirupsen/logrus@v1.2.0/exported.go
/go/pkg/mod/github.com/sirupsen/logrus@v1.2.0/alt_exit_test.go
/go/pkg/mod/github.com/sirupsen/logrus@v1.2.0/doc.go
....
.... there are a LOT of these -- everything you need to recreate the exec.
....
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/objects/6d
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/objects/6d/6af1a83abf0c816be79b131271c1f311d28613
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/objects/df
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/objects/df/1d582a728aec65edfe02b828f75d8a7def892b
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/objects/1f
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/objects/1f/e3cf3d5d10ef9e2d4145186c691ccce698195c
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/FETCH_HEAD
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/refs
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/refs/tags
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/refs/tags/v0.1.1
/go/pkg/mod/cache/vcs/b58cd1804573f08b6cfc86bbbad2960dd009cb14e98e8b74221958153f37a31b/refs/heads

Check out the Athens project (Aaron Schlesinger from Microsoft)

root@58be8a1015a8:~/mycat# go mod vendor
root@58be8a1015a8:~/mycat# ls
go.mod	go.sum	main.go  mycat	vendor
root@58be8a1015a8:~/mycat# ls vendor
github.com  golang.org	modules.txt

root@58be8a1015a8:~/mycat# find vendor
vendor
vendor/modules.txt
vendor/github.com
vendor/github.com/sirupsen
vendor/github.com/sirupsen/logrus
vendor/github.com/sirupsen/logrus/exported.go
vendor/github.com/sirupsen/logrus/doc.go
vendor/github.com/sirupsen/logrus/writer.go
vendor/github.com/sirupsen/logrus/hooks.go
vendor/github.com/sirupsen/logrus/CHANGELOG.md
................ lots of entries ...........
vendor/golang.org/x/sys/windows/security_windows.go
vendor/golang.org/x/sys/windows/syscall_windows.go
vendor/golang.org/x/sys/windows/memory_windows.go
vendor/golang.org/x/sys/PATENTS
root@58be8a1015a8:~/mycat# 

root@58be8a1015a8:~/mycat# cat go.sum
github.com/davecgh/go-spew v1.1.1 h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c=
github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/konsorten/go-windows-terminal-sequences v1.0.1 h1:mweAR1A6xJ3oS2pRaGiHgQ4OO8tzTaLawm8vnODuwDk=
github.com/konsorten/go-windows-terminal-sequences v1.0.1/go.mod h1:T0+1ngSBFLxvqU3pZ+m/2kptfBszLMUkC4ZK/EgS/cQ=
github.com/pmezard/go-difflib v1.0.0 h1:4DBwDE0NGyQoBHbLQYPwSUPoCMWR5BEzIk/f1lZbAQM=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/sirupsen/logrus v1.2.0 h1:juTguoYk5qI21pwyTXY3B3Y5cOTH3ZUyZCg1v/mihuo=
github.com/sirupsen/logrus v1.2.0/go.mod h1:LxeOpSwHxABJmUn/MG1IvRgCAasNZTLOkJPxbbu5VWo=
github.com/stretchr/objx v0.1.1/go.mod h1:HFkY916IF+rwdDfMAkV7OtwuqBVzrE8GR6GFx+wExME=
github.com/stretchr/testify v1.2.2 h1:bSDNvY7ZPG5RlJ8otE/7V6gMiyenm9RtJ7IUVIAoJ1w=
github.com/stretchr/testify v1.2.2/go.mod h1:a8OnRcib4nhh0OaRAV+Yts87kKdq0PP7pXfy6kDkUVs=
golang.org/x/crypto v0.0.0-20180904163835-0709b304e793 h1:u+LnwYTOOW7Ukr/fppxEb1Nwz0AtPflrblfvUudpo+I=
golang.org/x/crypto v0.0.0-20180904163835-0709b304e793/go.mod h1:6SG95UA2DQfeDnfUPMdvaQW0Q7yPrPDi9nlGo2tz2b4=
golang.org/x/crypto v0.0.0-20181112202954-3d3f9f413869 h1:kkXA53yGe04D0adEYJwEVQjeBppL01Exg+fnMjfUraU=
golang.org/x/crypto v0.0.0-20181112202954-3d3f9f413869/go.mod h1:6SG95UA2DQfeDnfUPMdvaQW0Q7yPrPDi9nlGo2tz2b4=
golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33 h1:I6FyU15t786LL7oL/hn43zqTuEGr4PN7F4XJ1p4E3Y8=
golang.org/x/sys v0.0.0-20180905080454-ebe1bf3edb33/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=
golang.org/x/sys v0.0.0-20181116161606-93218def8b18 h1:Wh+XCfg3kNpjhdq2LXrsiOProjtQZKme5XUx7VcxwAw=
golang.org/x/sys v0.0.0-20181116161606-93218def8b18/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=
// and from this list we can ask why is an entry included:
root@58be8a1015a8:~/mycat# go mod why github.com/pmezard/go-difflib
# github.com/pmezard/go-difflib
(main module does not need package github.com/pmezard/go-difflib)

root@58be8a1015a8:~/mycat# go mod why -m github.com/pmezard/go-difflib
# github.com/pmezard/go-difflib
gihub.com/davmaz/mycat
github.com/sirupsen/logrus
github.com/sirupsen/logrus.test
github.com/stretchr/testify/assert
github.com/pmezard/go-difflib/difflib

What if we want to use modules with an existing package?
root@58be8a1015a8:~/mycat# cd
root@58be8a1015a8:~# git clone https://github.com/campoy/embedmd
Cloning into 'embedmd'...
remote: Enumerating objects: 281, done.
remote: Total 281 (delta 0), reused 0 (delta 0), pack-reused 281
Receiving objects: 100% (281/281), 11.46 MiB | 5.55 MiB/s, done.
Resolving deltas: 100% (147/147), done.

root@58be8a1015a8:~# ls
embedmd  mycat
root@58be8a1015a8:~# cd embedmd/
root@58be8a1015a8:~/embedmd# ls
LICENSE  README.md  embedmd  integration_test.go  main.go  main_test.go  releases  sample

root@58be8a1015a8:~/embedmd# go mod init
go: creating new go.mod: module github.com/campoy/embedmd
root@58be8a1015a8:~/embedmd# ls
LICENSE  README.md  embedmd  go.mod  integration_test.go  main.go  main_test.go  releases  sample
root@58be8a1015a8:~/embedmd# cat go.mod
module github.com/campoy/embedmd

root@58be8a1015a8:~/embedmd# go get ./...
go: finding github.com/pmezard/go-difflib/difflib latest
root@58be8a1015a8:~/embedmd# cat go.mod
module github.com/campoy/embedmd

require github.com/pmezard/go-difflib v1.0.0

root@58be8a1015a8:~/embedmd# cat go.sum
github.com/pmezard/go-difflib v1.0.0 h1:4DBwDE0NGyQoBHbLQYPwSUPoCMWR5BEzIk/f1lZbAQM=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
root@58be8a1015a8:~/embedmd# go mod tidy
root@58be8a1015a8:~/embedmd# cat go.mod
module github.com/campoy/embedmd

require github.com/pmezard/go-difflib v1.0.0

root@58be8a1015a8:~/embedmd# go mod vendor
root@58be8a1015a8:~/embedmd# find vendor
vendor
vendor/modules.txt
vendor/github.com
vendor/github.com/pmezard
vendor/github.com/pmezard/go-difflib
vendor/github.com/pmezard/go-difflib/LICENSE
vendor/github.com/pmezard/go-difflib/difflib
vendor/github.com/pmezard/go-difflib/difflib/difflib.go

Now what if we use a package that DOES include vendoring?
root@58be8a1015a8:~/embedmd# cd
root@58be8a1015a8:~# git clone https://github.com/campoy/go-tooling-workshop
Cloning into 'go-tooling-workshop'...
remote: Enumerating objects: 1108, done.
remote: Total 1108 (delta 0), reused 0 (delta 0), pack-reused 1108
Receiving objects: 100% (1108/1108), 18.04 MiB | 7.05 MiB/s, done.
Resolving deltas: 100% (507/507), done.

root@58be8a1015a8:~# cd go-tooling-workshop/

root@58be8a1015a8:~/go-tooling-workshop# ls
1-source-code  2-building-artifacts  3-dynamic-analysis  CONTRIBUTING.md  Gopkg.lock  Gopkg.toml  LICENSE  README.md  congratulations.md  fancygopher.jpg  vendor
root@58be8a1015a8:~/go-tooling-workshop# go mod init
go: creating new go.mod: module github.com/campoy/go-tooling-workshop
go: copying requirements from Gopkg.lock
root@58be8a1015a8:~/go-tooling-workshop# cat go.mod
module github.com/campoy/go-tooling-workshop

require (
	github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa
	github.com/golang/example v0.0.0-20170213203618-0dea2d0bf907
	golang.org/x/sys v0.0.0-20170705195540-6faef541c737
)
root@58be8a1015a8:~/go-tooling-workshop# go mod tidy
go: finding github.com/golang/example v0.0.0-20170213203618-0dea2d0bf907
go: finding github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa
go: finding golang.org/x/sys v0.0.0-20170705195540-6faef541c737
go: downloading github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa
go: downloading golang.org/x/sys v0.0.0-20170705195540-6faef541c737
go: finding github.com/stretchr/testify/assert latest
go: finding github.com/pmezard/go-difflib/difflib latest
go: finding github.com/davecgh/go-spew/spew latest

Now tidy includes packages needed for testing.
root@58be8a1015a8:~/go-tooling-workshop# cat go.mod
module github.com/campoy/go-tooling-workshop

require (
	github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa
	github.com/davecgh/go-spew v1.1.1 // indirect
	github.com/pmezard/go-difflib v1.0.0 // indirect
	github.com/stretchr/testify v1.2.2 // indirect
	golang.org/x/sys v0.0.0-20170705195540-6faef541c737 // indirect
)
root@58be8a1015a8:~/go-tooling-workshop# cat go.sum
github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa h1:FJM2Xk7hjFoTKNi5vobJUTKQfz+eDl5xYZkbFycz2Nw=
github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa/go.mod h1:rmk17hk6i8ZSAJkSDa7nOxamrG+SP4P0mm+DAvExv4U=
github.com/davecgh/go-spew v1.1.1 h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c=
github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/pmezard/go-difflib v1.0.0 h1:4DBwDE0NGyQoBHbLQYPwSUPoCMWR5BEzIk/f1lZbAQM=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/stretchr/testify v1.2.2 h1:bSDNvY7ZPG5RlJ8otE/7V6gMiyenm9RtJ7IUVIAoJ1w=
github.com/stretchr/testify v1.2.2/go.mod h1:a8OnRcib4nhh0OaRAV+Yts87kKdq0PP7pXfy6kDkUVs=
golang.org/x/sys v0.0.0-20170705195540-6faef541c737 h1:oPvAbWbv3qHz0PNL9iiKiaFPKOGZjBHxuDo8QPU9CNA=
golang.org/x/sys v0.0.0-20170705195540-6faef541c737/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=

golang.org/x/sys v0.0.0-20170705195540-6faef541c737/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=
root@58be8a1015a8:~/go-tooling-workshop# rm -rf Gopkg.toml Gopkg.lock vendor
root@58be8a1015a8:~/go-tooling-workshop# ls
1-source-code  2-building-artifacts  3-dynamic-analysis  CONTRIBUTING.md  LICENSE  README.md  congratulations.md  fancygopher.jpg  go.mod  go.sum

root@58be8a1015a8:~/go-tooling-workshop# go get ./...
root@58be8a1015a8:~/go-tooling-workshop# cat go.sum
github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa h1:FJM2Xk7hjFoTKNi5vobJUTKQfz+eDl5xYZkbFycz2Nw=
github.com/Sirupsen/logrus v0.0.0-20170706134407-59d0ca41e5fa/go.mod h1:rmk17hk6i8ZSAJkSDa7nOxamrG+SP4P0mm+DAvExv4U=
github.com/davecgh/go-spew v1.1.1 h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c=
github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/pmezard/go-difflib v1.0.0 h1:4DBwDE0NGyQoBHbLQYPwSUPoCMWR5BEzIk/f1lZbAQM=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/stretchr/testify v1.2.2 h1:bSDNvY7ZPG5RlJ8otE/7V6gMiyenm9RtJ7IUVIAoJ1w=
github.com/stretchr/testify v1.2.2/go.mod h1:a8OnRcib4nhh0OaRAV+Yts87kKdq0PP7pXfy6kDkUVs=
golang.org/x/sys v0.0.0-20170705195540-6faef541c737 h1:oPvAbWbv3qHz0PNL9iiKiaFPKOGZjBHxuDo8QPU9CNA=
golang.org/x/sys v0.0.0-20170705195540-6faef541c737/go.mod h1:STP8DvDyc/dI5b8T5hshtkjS+E42TnysNCUPdjciGhY=
root@58be8a1015a8:~/go-tooling-workshop# go test ./...
?   	github.com/campoy/go-tooling-workshop/1-source-code/1-workspace/hello-vendor	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/1-workspace/pi	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/2-writing/eg-content	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/2-writing/eg-content/result	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/2-writing/errcheck	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/2-writing/hello	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/2-writing/tags	[no test files]
?   	github.com/campoy/go-tooling-workshop/1-source-code/2-writing/torename	[no test files]
?   	github.com/campoy/go-tooling-workshop/2-building-artifacts/exercise	[no test files]
?   	github.com/campoy/go-tooling-workshop/2-building-artifacts/web-server	[no test files]
ok  	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/2-testing	0.006s
ok  	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/2-testing/sum	0.029s
ok  	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/3-profiling	0.013s [no tests to run]
?   	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/3-profiling/webserver	[no test files]
?   	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/4-tracing/daisy	[no test files]
ok  	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/4-tracing/ping-pong	0.005s [no tests to run]
?   	github.com/campoy/go-tooling-workshop/3-dynamic-analysis/webserver	[no test files]

Modules are the way to go!

