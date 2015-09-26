Gryffin (beta)
==========

Gryffin is a large scale web security scanning platform. It is not yet another scanner. It was written to solve two specific problems with existing scanners: coverage and scale. 

Better coverage translates to fewer false negatives. Inherent scalability translates to capability of scanning, and supporting a large elastic application infrastructure. Simply put, the ability to scan 1000 applications today to 100,000 applications tomorrow by straightforward horizontal scaling.   

## Coverage
Coverage has two dimensions - one during crawl and the other during fuzzing. In crawl phase, coverage implies being able to find as much of the application footprint. In scan phase, or while fuzzing, it implies being able to test each part of the application for an applied set of vulnerabilities in a deep.

#### Crawl Coverage
Today a large number of web applications are template-driven, meaning the same code or path generates millions of URLs. For a security scanner, it just needs one of the millions of URLs generated by the same code or path. Gryffin's crawler does just that. 

##### Page Deduplication 
At the heart of Gryffin is a deduplication engine that compares a new page with already seen pages. If the HTML structure of the new page is similar to those already seen, it is classified as a duplicate and not crawled further.

##### DOM Rendering and Navigation
A large number of applications today are rich applications. They are heavily driven by client-side JavaScript. In order to discover links and code paths in such applications, Gryffin's crawler uses PhantomJS for DOM rendering and navigation.

#### Scan Coverage
As Gryffin is a scanning platform, not a scanner, it does not have its own fuzzer modules, even for fuzzing common web vulnerabilities like XSS and SQL Injection.

It's not wise to reinvent the wheel where you do not have to. Gryffin at production scale at Yahoo uses open source and custom fuzzers. Some of these custom fuzzers might be open sourced in the future, and might or might not be part of the Gryffin repository.

For demonstration purposes, Gryffin comes integrated with sqlmap and arachni. It does not endorse them or any other scanner in particular. 

The philosophy is to improve scan coverage by being able to fuzz for just what you need.

## Scale
While Gryffin is available as a standalone package, it's primarily built for scale. 

Gryffin is built on the publisher-subscriber model. Each component is either a publisher, or a subscriber, or both. This allows Gryffin to scale horizontally by simply adding more subscriber or publisher nodes.

## Operating Gryffin

### Pre-requisites 

1. Go 
2. PhantomJS, v2
3. Sqlmap (for fuzzing SQLi)
4. Arachni (for fuzzing XSS and web vulnerabilities)
5. NSQ , 
    - running lookupd at port 4160,4161
    - running nsqd at port 4150,4151
    - with `--max-msg-size=5000000`
6. Kibana and Elastic search, for dashboarding
    - listening to JSON over port 5000
    - Preconfigured docker image available in https://hub.docker.com/r/yukinying/elk/


### Installation

```
go get github.com/yahoo/gryffin/...
```

### Run

## TODO 

1. Mobile browser user agent
2. Preconfigured docker images 
3. Redis for sharing states across machines
4. Instruction to run gryffin (distributed or standalone)
5. Documentation for html-distance
6. Implement a JSON serializable cookiejar. 
7. Identify duplicate url patterns based on simhash result.

## Credits 

- Adonis Fung @ Yahoo, for the asynchronous phantomjs based crawler and DOM event navigator.
- [Simhash algorithm](http://www.cs.princeton.edu/courses/archive/spring04/cos598B/bib/CharikarEstim.pdf) by Moses Charikar
- Simhash implementation provided by [mfonda/simhash](https://github.com/mfonda/simhash). 
- [Sqlmap](http://sqlmap.org/) 
- [Arachni](http://www.arachni-scanner.com/)


## Licence

Code licensed under the BSD-style license. See LICENSE file for terms.
