# Javascript drum kit 
javascript30 https://javascript30.com

完成品
https://telsaiori.github.io/javascript_drum_kit/index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>JS Drum Kit</title>
<link rel="stylesheet" href="style.css">
</head>
<body>


<div class="keys">
<div data-key="65" class="key">
<kbd>A</kbd>
<span class="sound">clap</span>
</div>
<div data-key="83" class="key">
<kbd>S</kbd>
<span class="sound">hihat</span>
</div>
<div data-key="68" class="key">
<kbd>D</kbd>
<span class="sound">kick</span>
</div>
<div data-key="70" class="key">
<kbd>F</kbd>
<span class="sound">openhat</span>
</div>
<div data-key="71" class="key">
<kbd>G</kbd>
<span class="sound">boom</span>
</div>
<div data-key="72" class="key">
<kbd>H</kbd>
<span class="sound">ride</span>
</div>
<div data-key="74" class="key">
<kbd>J</kbd>
<span class="sound">snare</span>
</div>
<div data-key="75" class="key">
<kbd>K</kbd>
<span class="sound">tom</span>
</div>
<div data-key="76" class="key">
<kbd>L</kbd>
<span class="sound">tink</span>
</div>
</div>

<audio data-key="65" src="sounds/clap.wav"></audio>
<audio data-key="83" src="sounds/hihat.wav"></audio>
<audio data-key="68" src="sounds/kick.wav"></audio>
<audio data-key="70" src="sounds/openhat.wav"></audio>
<audio data-key="71" src="sounds/boom.wav"></audio>
<audio data-key="72" src="sounds/ride.wav"></audio>
<audio data-key="74" src="sounds/snare.wav"></audio>
<audio data-key="75" src="sounds/tom.wav"></audio>
<audio data-key="76" src="sounds/tink.wav"></audio>

<script>


function playSound(e) {
const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
if (!audio) return; // stop the fucntion from running all together
audio.currentTime = 0; // rewind to the start
audio.play();
key.classList.add('playing');
}
function removeTransition(e) {
console.log(e);
if (e.propertyName !== 'transform') return; // skip it if it's not a transform
this.classList.remove('playing');
}

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
window.addEventListener('keydown', playSound);

</script>

</body>
</html>

```
首先要先可以抓到現在使用者按了哪一個key,和相對應的聲音是什麼
所以使用addEventListener來偵測按下key的動作
playSound裡面用了querySelector來抓出相對應的key和聲音
```
const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
```
這時候如果用console.log(e);的話發現e會代表現在發生的keyboard事件,可以用她抓到keycode得知使用者按了哪個鍵
而上面的html本來各個按鍵和聲音的data就是照著真實的keycode去寫的,因此可以對照在一起
```
if (!audio) return;
```
上面這行code是用來判斷如果使用者按了不支援的字母的時候直接中斷function,既然找不到audio也等於不支援
```
audio.currentTime = 0;
```
如果不加上面這行的話,如果你想連打某個key讓他連續發出聲音是做不到的,因為他會等現在的聲音播完然後再繼續
currentTime可以指定audio/video播放的當前位置,靠著每次都把它從頭開始播,就不會等到播完了才繼續




然後因為我們會再相對應字母的地方新增playing class,並改變他的樣式,所以必須要寫程式讓他會自動變回來本來的樣子

```
.key {
...
transition:all .07s;
...
}

.playing {
transform:scale(1.1);
border-color:#ffc600;
box-shadow: 0 0 10px #ffc600;
}
```
(key有砍掉其他css)
key設定了transition來指定改變外觀需要花多少時間,而playing設定了transform:scale讓元素放大

```
const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
```
上面keys先抓出所有key class,然後再用forEach一個一個去偵測他有沒有transitionend的event

想當然如果這時候在removeTransition去印出e的話,得到的就是所有正在轉場的event,全部會列出6個
```
transform:scale(1.1); //一個
border-color:#ffc600; //上下左右各一個
box-shadow: 0 0 10px #ffc600; //一個
```
最後用this（在這邊會是觸發事件的那個key class)來remove掉playing class
就完成了







