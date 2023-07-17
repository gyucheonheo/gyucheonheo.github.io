---
layout: post
title: Javascript timer goes slow
subtitle: 
tags: [javascript, web, webworker, timer]
comments: true
---

Recently, I was testing a multi-factor authentication code expiry. As its expiry is minutes, I hide the tab and attended other meetings.
A few minutes later when I open that tab, I observe one crazy situation where a timer went slow! After a few more times of attempts, I confirmed that it goes slow when it's hidden and concluded that this javascript is de-prioritized because of memory throttling!

I thought that my solution was very close to achieve it but 
TBD..
Still figuring out it's correlation between visibility change and 



``` javascript
// timer_worker.js

self.onmessage = function(event) {
  if(event.data.action === 'timer_start') {
    const duration = event.data.duration;
    const interval = 1000; // 1 second

    let remaining_time = duration;
    const timer = setInterval( () => {
      remaining_time -= interval;
      if(remaining_time <= 0){
        self.postMessage({
          action: 'complete',
          remaining_time: 0
        })
      } else {
        self.postMessage({
          action: 'tick',
          remaining_time: remaining_time    
        }) 
      }
    }, interval)
  }
}
```

``` html
// index.html
<script src="time_worker.js"></script>
<script>
  const worker = new Worker("time_worker.js");

  const duration = 60000; // 1 min

  worker.postMessage({ action: 'timer_start', duration: duration})

  worker.onmessage = function(event) {
    if(event.data.action === 'tick') {
      console.log(event.data.remaining_time);
    } else {
      console.log("00:00");
    }
  }
</script>
```
