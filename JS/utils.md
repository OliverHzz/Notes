```
/**
 * 倒计时(传入毫秒)
 * @param {new Date.getTime()} times 毫秒
 */
countDown (times) {
  times /= 1000
  let hour, minute, second
  let timer = setInterval(() => {
    // 倒计时结束
    if (times < 0) {
      clearInterval(timer)
      return false
    }
    
    times --
    
    hour = Math.floor(times / (60 * 60))
    minute = Math.floor(times / 60) - (hour * 60)
    second = Math.floor(times) - (hour * 60 * 60) - (minute * 60)
    
    // 更新状态
    console.log(
        hour: hour.toString().padStart(2, 0),
        minute: minute.toString().padStart(2, 0),
        second: second.toString().padStart(2, 0)
    )
  }, 1000)
}
```