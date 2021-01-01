# litle_scripts

```js
let pathname = decodeURI(location.pathname);
const regex = /\/ru\/библиотека\/видео\//gm;

if(pathname.match(regex)) {

	function finder() {
		if(!document.querySelector('video')) {
			setTimeout(finder, 100);
		} else {
			doPlayer();
		}
	}
	finder();
}

function doPlayer() {
	let player = document.querySelector('#videoPlayerInstance');
	let video = player.querySelector('video');

	function togglePlay() {
		const method = video.paused ? 'play' : 'pause';
		video[method]();
	}
  
	function changeSpeed(e) {
		let div = document.createElement('div');
		let setSpeed = video.playbackRate;

		div.style.position = 'absolute';
		div.style.height = '100%';
		div.style.width = '100%';
		div.style.display = 'flex';
		div.style.justifyContent = 'center';
		div.style.alignItems = 'center';
		div.style.background = 'rgba(0,0,0,.6)';
		div.style.color = '#fff';
		div.style.fontSize = '52px';
		div.style.top = '0';
		div.style.left = '0';



		let changeV   = function () {
			setSpeed = + setSpeed.toFixed(1);
			if(setSpeed > 2 || setSpeed < 0.5) {
				return;
			}


			div.innerHTML = `<div>${setSpeed}</div>`;
			video.playbackRate = setSpeed;
			document.querySelector('#videoPlayerInstance>div').appendChild(div);
			setTimeout(function(){
				div.remove()
			}, 500)
		}


		if(e.code === 'BracketLeft') {
			setSpeed -= .1
			changeV();
		} else if(e.code === 'BracketRight') {
			setSpeed += .1
			changeV();
		}
	}

	function changeVolume(e) {
		let setVolume = video.volume;
		let changeV   = function () {
    	setVolume = + setVolume.toFixed(1);
      if(setVolume > 1 || setVolume < 0) {
        return;
      }
      video.volume = setVolume;
    }

		if(e.code === 'ArrowUp') {
			setVolume += .1
      changeV();
		} else if(e.code === 'ArrowDown') {
			setVolume -= .1
      changeV();
    } else if(e.code === 'KeyM') {
      if(setVolume === 0) {
      	setVolume = .5
      } else {
      	setVolume = 0
      }
      changeV();
    }
   

    
	}
  
  function toggleSubtitles(e) {
    if(e.code === 'KeyS') {
    	document.querySelector('.vjs-subtitles-toggle').click();
    }
  }
  
	// play/pause with space-bar and skip with left/right arrow
	function keyNav(e) {
		if(e.code === "Space") {
			togglePlay();
		} else if (e.code === "ArrowRight") {
			video.currentTime += 5;
		} else if (e.code === "ArrowLeft") {
			video.currentTime -= 5;
		}
	}

	// fullscreen function
	function toggleFullscreen(e) {
		if(e.code === 'KeyF') {
			if (!document.fullscreenElement && !document.mozFullScreenElement &&
				!document.webkitFullscreenElement && !document.msFullscreenElement) {
				if (player.requestFullscreen) {
					player.requestFullscreen();
				} else if (player.msRequestFullscreen) {
					player.msRequestFullscreen();
				} else if (player.mozRequestFullScreen) {
					player.mozRequestFullScreen();
				} else if (player.webkitRequestFullscreen) {
					player.webkitRequestFullscreen(Element.ALLOW_KEYBOARD_INPUT);
				}
			} else {
				if (document.exitFullscreen) {
					document.exitFullscreen();
				} else if (document.msExitFullscreen) {
					document.msExitFullscreen();
				} else if (document.mozCancelFullScreen) {
					document.mozCancelFullScreen();
				} else if (document.webkitExitFullscreen) {
					document.webkitExitFullscreen();
				}
			}
		}
		
	}
  
  player.onkeydown = function(e){
  	e.preventDefault();
    keyNav(e);
    toggleSubtitles(e);
    changeVolume(e);
    toggleFullscreen(e);
    changeSpeed(e);
  }
}
```
