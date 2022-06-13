![contributions welcome](./img/contributions.svg) <a href="https://twitter.com/kh4sh3i">
    <img src="https://img.shields.io/badge/author-@kh4sh3i-orange.svg?style=square&logo=twitter">
  </a>
  
  
# FFUF Tricks
Describe how to use ffuf different options with examples


![maxresdefault](./img/ffuf.png)


### So what is ffuf?

Ffuf(fuzz faster u fool) is a great tool used for fuzzing. It has become really popular lately with bug bounty hunters/penetration tester. It is written in Go language.For this you can fuzz a large amount of words within a minute.


## wordlist:

Option name: -w

Use wordlist on ffuf for more affectively fuzzing. I use SecLists-master for example.
```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ
```

## Fuff with all domain

This is a common problem for beginners that they don’t know how to use fuff in all of their collected subdomains as fuff has no default option for list of domains like dirsearch.

```
cat live.txt | xargs -I@ sh -c 'ffuf -w wordlists.txt -u @/FUZZ -mc 200'
```

## Filtering:


Option name: -fc

If you don’t want to see any kind of specific status code then you can just filter them.

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -fc 401,403,404
```
Comma-separated list of codes and ranges


## Recursion:

Option name: -recursion

With this option, it tries to find all possible dir accordingly your given wordlist. Let me explain if ffuf find /index.php dir then it fuzz it again with /index.php/wordlist. 
Suppose it finds/index.php/configtest.php then it fuzz it again like this /index.php/configtest.php/wordlist.

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -recursion
```

## Recursion-Depth:

Option name: -recursion-depth

By default recursion depth level is 0.with this you set how many specific numbers of dir it find for you. Like 2,3 or 4 etc.

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -recursion-depth 2
```
Here you see I set recursion-depth 2. Now ffuf find 2 dir basis of my wordlist if these dir are available on the targeted website then stop.


## Extention:

Option name: -e

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -e .html,.php,.txt,.pdf
```
Sometimes it gives you valuable information. Which is maybe goldmine on your penetration testing/bug hunting.For this, you have to choose extension base on your target.


## Silent:

Option name: -s

If you just print the result and don’t see any kind of fuzzing process on your terminal then use silent option.

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -s
```

## Output:

Choose one -of json, ejson, html, md, csv

I generally use | tee for result output. But if you want to get output on GUI(graphical user interface) for your better understand/client demand then your CM is.

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -of html -o result
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ | tee result.txt
```

## Subdomain Enumeration

```
./ffuf -w /root/Desktop/wordlist.txt -u http://FUZZ.ab.com -of html -o result
```
Remember use http:// protocol after "-u" because sometimes many subdomains do not run over https.

## Automatically calibrate filtering

Option name: -ac

So this is a very useful thing, while Directory Bruteforce you may see sometimes we see lots of same length status code like 403,401 etc that means the output isn’t that much useful as they treat all of our directory bruteforce wordlists at the same length. This is problematic when you have a big wordlist and the same length 403 repats 20000 or 30000 times(think about your messy output) So what should you do? should you use -fc option in your command for filtering 403 then you may miss some sensitive directory.
In this time -ac options comes into the picture. This option automatically removes the same length dir and gives you a nice and clean output.

```
./ffuf -w /root/Desktop/wordlist.txt -u http://FUZZ.ab.com -ac
```

## Throttle(Last one but an important one)

Option name: -rate 2 (set your number 2,3 etc)

This is very useful because with this you throttle/delay your request. As you know ffuf is very fast tool with that a large number of wordlist makes much noise on the server which may cause to block your IP,Dos,Slow down the server etc. To avoid this you can use -rate and your CM is.

```
./ffuf -w raft-large-directories.txt -u https://xyz.com/FUZZ -rate 2
```
rate 2 means two requests per second. You can also customize the number.



### Here are some other useful options on ffuf:

* timeout → HTTP request timeout in seconds (default: 10)
* -V → Show version information (default: false/off)
* -t → Number of concurrent threads(default: 40)
* -v → Verbose/details output,printing full URL and redirect location (if any) with the results (default:false/off)
* -mc → Match HTTP status codes, or "all" for everything (default: 200,204,301,302,307,401,403)
* mode → Multi-wordlist operation mode.Available modes: clusterbomb, pitchfork (default: clusterbomb(1 to 1,2 to 2)


#### Reffrence
[ffuf](https://github.com/ffuf/ffuf)
  

