# Raft

This is an instructional implementation of the Raft distributed consensus
algorithm in Go. It's accompanied by a series of blog posts:

* Part 0: Introduction
* Part 1: Elections (coming soon)
* Part 2: Commands and log replication (coming soon)
* Part 3: Persistence and optimizations (coming soon)

Each of the `partN` directories in this repository is the complete source code
for Part N of the blog post series (except Part 0, which is introductory and has
no code). There is a lot of duplicated code between the different `partN`
directories - this is a conscious design decision. Rather than abstracting and
reusing parts of the implementation, I opted for keeping the code as simple
as possible. Each directory is completely self contained and can be read and
undestood in isolation. Using a graphical diff tool to see the deltas between
the parts can be instructional.

## How to use this repository

You can read the code, but I'd also encourage you to run tests and observe the
logs they print out. The repository contains a useful tool for visualizing
output. Here's a complete usage example:

```
$ cd part1
$ go test -v -race -run TestElectionFollowerComesBack |& tee /tmp/raftlog
... logging output
... test should PASS
$ go run ../tools/raft-testlog-viz/main.go < /tmp/raftlog
PASS TestElectionFollowerComesBack map[0:true 1:true 2:true TEST:true] ; entries: 150
... Emitted file:///tmp/TestElectionFollowerComesBack.html

PASS
```

Now open `file:///tmp/TestElectionFollowerComesBack.html` in your browser.
You should see something like this:

![Image of log browser](https://github.com/eliben/raft/blob/master/raftlog-screenshot.png)

Scroll and read the logs from the servers, noticing state changes (highlighted
with colors). Feel free to add your own `cm.dlog(...)` calls to the code to
experiment and print out more details.

## Changing and testing the code

Each `partN` directory is completely independent of the others, and is its own
Go module. The Raft code itself has no external dependencies; the only `require`
in its `go.mod` is for a package that enables goroutine leak testing - it's only
used in tests.

To work on `part2`, for example:

```
$ cd part2
... make code changes
$ go test -race ./...
```

Depending on the part and your machine, the tests can take up to a minute to
run. Feel free to enable verbose logging with ``-v``, and/or used the provided
``dotest.sh`` script to run specific tests with log visualization.
