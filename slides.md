---
title-prefix: Six Feet Up
pagetitle: Practical Python Async in 42 Lines of Code
author: Calvin Hendryx-Parker, CTO, Six Feet Up
author-meta:
    - Calvin Hendryx-Parker
date: IndyPy 2021
date-meta: 2021
keywords:
    - Python
    - Programming
    - Code
---

# Practical Python Async in 42 Lines of Code {.semi-filtered data-background-image="images/david-clode-o8C6UFpqC4s-unsplash.jpg"}
#### Calvin Hendryx-Parker, CTO
#### Six Feet Up
#### September 2021 IndyPy Meetup

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@davidclode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">David Clode</a> on <a href="https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# How it started {data-background-image="images/david-clode-Q2S8rrSzz1U-unsplash.jpg"}

```python
def grab_twitter_handles(url):
    res = requests.get(url)
    soup = BeautifulSoup(res.text, features='html.parser')
    twit_links = []
    for link in soup('a', href=re.compile('twitter.com')):
        twit_links.append(urlparse(link['href']).path[1:])

    print("\n".join(twit_links))
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@davidclode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">David Clode</a> on <a href="https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# Cool {data-background-image="images/julian-hochgesang-3-y9vq8uoxk-unsplash.jpg"}

```{data-line-numbers="3"}
/home/calvin/Projects/ScrapeTwitter/venv/bin/python /home/calvin/Projects/ScrapeTwitter/main.py https://techfieldday.com/event/nginxsprint21/
intent/tweet,VirtualisedReal,CalvinHP,DarrelClute,KleeGeek,JABenedicic,KatiLehmuskoski,Ned1313,NickJanetakis,TechStringy,BenTGage,MrMattGarvin,SFoskett,TechFieldDay
Downloaded speakers in 0.7954 seconds

Process finished with exit code 0
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@julianhochgesang?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Julian Hochgesang</a> on <a href="https://unsplash.com/s/photos/speed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# Events don't always list all speakers in one page {.r-fit-text data-background-image="images/markus-spiske-iar-afB0QQw-unsplash.jpg"}

~~~ {.python data-line-numbers="|7-9"}
def grab_speakers(url):
    r = requests.get(url)
    text = r.text
    soup = BeautifulSoup(text, features="html.parser")
    speakers = soup.select("h3.talk-title a")
    twit_links = []
    for speaker in speakers:
        if link := grab_twitter(speaker.attrs["href"]):
            twit_links += link
    print(",".join(twit_links))
~~~

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Markus Spiske</a> on <a href="https://unsplash.com/s/photos/matrix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# Then grab each speaker {data-background-image="images/mila-tovar-DjjhXIo1dc8-unsplash.jpg"}

~~~ {.python data-line-numbers="|3"}
def grab_twitter(url):
    twit_links = []
    r = requests.get(f"{BASE_URL}{url}")
    text = r.text
    soup = BeautifulSoup(text, features="html.parser")
    for link in soup("a", href=re.compile("twitter.com")):
        twit_links.append(urlparse(link["href"]).path[1:])
    return twit_links
~~~

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@milatovar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Mila Tovar</a> on <a href="https://unsplash.com/s/photos/tree-branches?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::


# Not as cool {data-background-image="images/nick-abrams-FTKfX3xZIcc-unsplash.jpg"}

```{data-line-numbers="3"}
/home/calvin/.virtualenvs/scrape_twitter_handles/bin/python /home/calvin/Projects/scrape_twitter_handles/main.py
aaronbassett,adamghill,adamchainz,amakarudze,benjy,calvinhp,mrchrisadams,evildmp,davidwobrock,dhilipsiva,emmadelescolle,xahteiwi,gcdeshpande,be_haki,israelst,jezdez,jvieilledent,ephes,shezoidic/,tammmri,m_holtermann,gasmanic,meredydd,pauloxnet,patrick91,aspigirlcodes,stefanbaerisch,guettli
Downloaded speakers in 24.3744 seconds

Process finished with exit code 0
```
::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@nbabrams?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Nick Abrams</a> on <a href="https://unsplash.com/s/photos/slow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# Let's add some Async {data-background-image="images/sharon-mccutcheon-TMwHpCrU8D4-unsplash.jpg"}

```diff
 def grab_twitter(url):
     twit_links = []
-    r = requests.get(f"{BASE_URL}{url}")
+    r = httpx.get(f"{BASE_URL}{url}")
     text = r.text
     
... 

 def grab_speakers(url):
-    r = requests.get(url)
+    r = httpx.get(url)
     text = r.text
     soup = BeautifulSoup(text, features="html.parser")
     speakers = soup.select("h3.talk-title a")
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@sharonmccutcheon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sharon McCutcheon</a> on <a href="https://unsplash.com/s/photos/fairy-dust?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
focused on performance, lower latency across requests, reduced CPU and round-trips and reduced net congestion
support cookie persistence across requests and HTTP/2
:::

# Not really better {data-background-image="images/alex-blajan-qMWgy4LpaIs-unsplash.jpg"}
## but now we have an Async lib to play with

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@alexb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Alex Blăjan</a> on <a href="https://unsplash.com/s/photos/slow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# httpx

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The HTTPX 1.0 pre-release is now available...<br><br>* Feature parity with requests<br>* Integrated command line client<br>* HTTP/1.1 and HTTP/2 support<br>* Sync and Async support<br>* More still to come...<br><br>You can give it a whirl now with:<br><br>$ pip install httpx[cli,http2] --pre<br>$ httpx --help <a href="https://t.co/S4Zf9BPgjP">pic.twitter.com/S4Zf9BPgjP</a></p>&mdash; Tom Christie (@starletdreaming) <a href="https://twitter.com/starletdreaming/status/1437709389784309760?ref_src=twsrc%5Etfw">September 14, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

:::notes
Tom Christie is also the creator of DRF, starlette and uvicorn

One issue with his install instructions and here's how to fix it
:::

# Drop in replacement for `requests`... almost {data-background-image="images/roberto-sorin-ZZ3qxWFZNRg-unsplash.jpg"}

* `httpx` does not follow redirects by default
* `requests.Session()` is pretty much a `httpx.Client()`
* `response.url` will be a `URL()` instance instead of a `str()`
* `httpx` uses `HTTPCore` vs `urllib3`
* Request body on `GET`, `DELETE`, `HEAD` and `OPTIONS`

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@roberto_sorin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Roberto Sorin</a> on <a href="https://unsplash.com/s/photos/recycle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes 
just a few of the differences, others you may not run into

you can work around the request body diff by using `httpx.request()` and specify a content, files, data or json arg
:::

# Bonus CLI! {data-background-image="images/cli.png"}

```bash
$ pipx install "httpx[cli,http2]==1.0.0b0"
  installed package httpx 1.0.0b0, Python 3.9.7
  These apps are now globally available
    - httpx
done! ✨ 🌟 ✨
```

<https://www.python-httpx.org/>

:::notes
Not quite a powerful as httpie, but supports http/2

Demo installing and grabbing something from the petstore.
:::

# Make it Async for real {data-background-image="images/marc-sendra-martorell--Vqn2WrfxTQ-unsplash.jpg"}

```diff
+import asyncio
...
-def grab_twitter(url):
+async def grab_twitter(url):
     twit_links = []
-    r = httpx.get(f"{BASE_URL}{url}")
+    async with httpx.AsyncClient() as client:
+        r = await client.get(f"{BASE_URL}{url}")
     text = r.text
     
... 

-def grab_speakers(url):
-    r = httpx.get(url)
+async def grab_speakers(url):
+    async with httpx.AsyncClient() as client:
+        r = await client.get(url)
     text = r.text
     soup = BeautifulSoup(text, features="html.parser")
     speakers = soup.select("h3.talk-title a")
-    twit_links = []
-    for speaker in speakers:
-        if link := grab_twitter(speaker.attrs["href"]):
-            twit_links += link
-    print(",".join(twit_links))
+    twit_links = await asyncio.gather(
+        *[grab_twitter(speaker.attrs["href"]) for speaker in speakers]
+    )
+    # flatten and de-dup
+    handles = {item for elem in twit_links for item in elem if item}
+    print(",".join(handles))
``` 

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@marcsm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Marc Sendra Martorell</a> on <a href="https://unsplash.com/s/photos/speed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
By default httpx in synchronous

To make asynchronous requests, you'll need an AsyncClient.
:::

# Async Context Manager {data-background-image="images/bofu-shaw-yty30exygSI-unsplash.jpg"}

~~~ {.python}
    async with httpx.AsyncClient() as client:
        r = await client.get(url)
~~~

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@hikeshaw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Bofu Shaw</a> on <a href="https://unsplash.com/s/photos/speed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
Clients will use HTTP connection pooling to reuse the TCP connection

You may want to add exception handling, and `httpx` has the same `requests` api for `response.raise_for_status()`
:::

# `asyncio.gather()` {data-background-image="images/david-klein-eHRokRBGR-w-unsplash.jpg"}

~~~ {.python}
    twit_links = await asyncio.gather(
        *[grab_twitter(speaker.attrs["href"]) for speaker in speakers]
    )
~~~

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@diklein?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">David Klein</a> on <a href="https://unsplash.com/s/photos/train-station-tracks-lines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
The real async magic here

The tasks are passed a positional args, hence the splat to explode the list

gather collects any awaitable in the list and schedules them as `Task`s

You can control how exceptions are handedled with the `return_exceptions` arg, it is `False` by
default, other tasks will continue, but the exception is immediately propagated. `True` for all tasks 
to be treated as success and return the exceptions in the result list.
:::

# Make Sure Your Tasks are Async {data-background-image="images/nick-fewings-4pZu15OeTXA-unsplash.jpg"}

```python
async def grab_twitter(url):
    twit_links = []
    async with httpx.AsyncClient() as client:
        r = await client.get(f"{BASE_URL}{url}")
    text = r.text
    soup = BeautifulSoup(text, features="html.parser")
    for link in soup("a", href=re.compile("twitter.com")):
        twit_links.append(urlparse(link["href"]).path[1:])
    return twit_links
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@jannerboy62?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Nick Fewings</a> on <a href="https://unsplash.com/s/photos/warning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
any sync methods can block your task and other tasks in the event loop

Calling a tag is like calling `find_all()` and the `BeautifulSoup()` object acts like a `Tag()`
:::

# Now we run it with asyncio... {data-background-image="images/miguel-a-amutio-Y0woUmyxGrw-unsplash.jpg"}

```diff 
 if __name__ == "__main__":
     tic = time.perf_counter()
-    grab_speakers(f"{BASE_URL}{SPEAKER_PATH}")
+    asyncio.run(grab_speakers(f"{BASE_URL}{SPEAKER_PATH}"))
     toc = time.perf_counter()
     print(f"Downloaded speakers in {toc - tic:0.4f} seconds")
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@amutiomi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Miguel A. Amutio</a> on <a href="https://unsplash.com/s/photos/run?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
Now we use `asyncio.run()` to call our freshly ported async methods.

httpx also support `trio` as an async environment, never used it, but it is based on the principles of [structured
concurrency](https://en.wikipedia.org/wiki/Structured_concurrency)
:::

# And Rejoice! {data-background-image="images/william-white-TZCppMjaOHU-unsplash.jpg"}

```{data-line-numbers="3"}
/home/calvin/.virtualenvs/scrape_twitter_handles/bin/python /home/calvin/Projects/scrape_twitter_handles/main.py
pauloxnet,be_haki,patrick91,meredydd,davidwobrock,stefanbaerisch,gasmanic,guettli,gcdeshpande,emmadelescolle,amakarudze,evildmp,ephes,tammmri,shezoidic/,mrchrisadams,xahteiwi,calvinhp,jezdez,adamchainz,aspigirlcodes,dhilipsiva,m_holtermann,aaronbassett,israelst,adamghill,benjy,jvieilledent
Downloaded speakers in 2.8114 seconds

Process finished with exit code 0
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@wrwhite3?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">William White</a> on <a href="https://unsplash.com/s/photos/cheer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# Add Exception Handling {data-background-image="images/sarah-kilian-52jRtc2S_VE-unsplash.jpg"}

```python
try:
    response = httpx.get("https://www.example.com")
    response.raise_for_status()
except httpx.HTTPError as exc:
    print(f"HTTP Exception for {exc.request.url} - {exc}")
```

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@rojekilian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sarah Kilian</a> on <a href="https://unsplash.com/s/photos/error?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
an HTTP error is going to fail silently in our code
:::

# Easy Debugging with `httpx` {data-background-image="images/etienne-girardet-T_fYMj6H-pE-unsplash.jpg"}

Set the `HTTPX_LOG_LEVEL` environment variable to `debug`

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@etiennegirardet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Etienne Girardet</a> on <a href="https://unsplash.com/s/photos/logging?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

:::notes
httpx also supports setting other vars for SSL certs and proxies
:::

# Resources {data-background-image="images/bryson-hammer-JZ8AHFr2aEg-unsplash.jpg"}

* <https://docs.python.org/3/library/asyncio-task.html#running-tasks-concurrently>
* <https://beautiful-soup-4.readthedocs.io/>
* <https://www.python-httpx.org/>
* <https://pypa.github.io/pipx/>
* <https://github.com/calvinhp/scrape_twitter_handles>

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@trhammerhead?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Bryson Hammer</a> on <a href="https://unsplash.com/s/photos/chain-links?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::

# Questions? {data-background-image="images/jon-tyson-hhq1Lxtuwd8-unsplash.jpg"}

#### <calvin@sixfeetup.com>

[`@calvinhp`](https://twitter.com/calvinhp)

::::::::::::::{.credits}
Photo by <a href="https://unsplash.com/@jontyson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jon Tyson</a> on <a href="https://unsplash.com/s/photos/questions?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
::::::::::::::
