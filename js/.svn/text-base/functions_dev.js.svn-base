// variables
var $window = $(window), gardenCtx, gardenCanvas, $garden, garden;
var clientWidth = $(window).width();
var clientHeight = $(window).height();

$(function () {
    // setup garden
	$loveHeart = $("#loveHeart");
	var offsetX = $loveHeart.width() / 2;
	var offsetY = $loveHeart.height() / 2 - 55;
    $garden = $("#garden");
    gardenCanvas = $garden[0];
	gardenCanvas.width = $("#loveHeart").width();
    gardenCanvas.height = $("#loveHeart").height()
    gardenCtx = gardenCanvas.getContext("2d");
    gardenCtx.globalCompositeOperation = "lighter";
    garden = new Garden(gardenCtx, gardenCanvas);
	
	$("#content").css("width", $loveHeart.width() + $("#code").width());
	$("#content").css("height", Math.max($loveHeart.height(), $("#code").height()));
	$("#content").css("margin-top", Math.max(($window.height() - $("#content").height()) / 2, 10));
	$("#content").css("margin-left", Math.max(($window.width() - $("#content").width()) / 2, 10));

    // renderLoop
    setInterval(function () {
        garden.render();
    }, Garden.options.growSpeed);
});

/*
$(window).resize(function() {
    var newWidth = $(window).width();
    var newHeight = $(window).height();
    if (newWidth != clientWidth && newHeight != clientHeight) {
        location.replace(location);
    }
});
*/

var getHeartPoint_mode = 0;
var getHeartPoint_leftbrow_x = -200;
var getHeartPoint_rightbrow_x = 100;
var getHeartPoint_nose_t = 0;
var getHeartPoint_leftnostril_y = 75;
var getHeartPoint_rightnostril_y = 75;
var getHeartPoint_mouth_x = -75;
function getHeartPoint() {
	switch (getHeartPoint_mode) {
		case 3: // 左眉
			var x = getHeartPoint_leftbrow_x;
			var y = -0.5 * (-Math.abs(x + 150)) - 150;
			getHeartPoint_leftbrow_x += 2;
			if (getHeartPoint_leftbrow_x > -100) {
				++getHeartPoint_mode;
			}
			return new Array(offsetX + x, offsetY + y);
		case 4: // 右眉
			var x = getHeartPoint_rightbrow_x;
			var y = 0.5 * (Math.abs(x - 150)) - 150;
			getHeartPoint_rightbrow_x += 2;
			if (getHeartPoint_rightbrow_x > 200) {
				++getHeartPoint_mode;
			}
			return new Array(offsetX + x, offsetY + y);
		case 0: // 鼻子
			var x = 150 * (1 - Math.cos(getHeartPoint_nose_t)) * Math.sin(getHeartPoint_nose_t);
			var y = -110 * (1 - Math.cos(getHeartPoint_nose_t)) * Math.cos(getHeartPoint_nose_t);
			getHeartPoint_nose_t += Math.PI / 180;
			if (getHeartPoint_nose_t >= 2 * Math.PI) {
				++getHeartPoint_mode;
			}
			return new Array(offsetX + x, offsetY + y);
		case 1: // 左鼻孔
			var y = getHeartPoint_leftnostril_y;
			var x = -100 + Math.abs(y - 100) * 0.6;
			getHeartPoint_leftnostril_y += 2;
			if (getHeartPoint_leftnostril_y > 125) {
				++getHeartPoint_mode;
			}
			return new Array(offsetX + x, offsetY + y);
		case 2: // 右鼻孔
			var y = getHeartPoint_rightnostril_y;
			var x = 100 - Math.abs(y - 100) * 0.6;
			getHeartPoint_rightnostril_y += 2;
			if (getHeartPoint_rightnostril_y > 125) {
				++getHeartPoint_mode;
			}
			return new Array(offsetX + x, offsetY + y);
		case 5: // 嘴巴
			var x = getHeartPoint_mouth_x;
			var y = 300;
			if (x < -40 || x > 40) {
				y -= Math.abs(x) * 0.2;
			}
			getHeartPoint_mouth_x += 2;
			if (getHeartPoint_mouth_x > 75) {
				++getHeartPoint_mode;
			}
			return new Array(offsetX + x, offsetY + y);
		case 6: // 左脸颊
			var x = -300;
			var y = 100;
			++getHeartPoint_mode;
			return new Array(offsetX + x, offsetY + y);
		case 7: // 右脸颊
			var x = 300;
			var y = 100;
			++getHeartPoint_mode;
			return new Array(offsetX + x, offsetY + y);
		default:
			return null;
	}
}

var heart = new Array();

function startHeartAnimation() {
	var interval = 90;
	
	var animationTimer = setInterval(function () {
		var bloom = getHeartPoint();
		if (null == bloom) {
			clearInterval(animationTimer);
			showMessages();
			return;
		}
		
		var draw = true;
		for (var i = 0; i < heart.length; i++) {
			var p = heart[i];
			var distance = Math.sqrt(Math.pow(p[0] - bloom[0], 2) + Math.pow(p[1] - bloom[1], 2));
			if (distance < Garden.options.bloomRadius.max * 2) {
				draw = false;
				break;
			}
		}
		if (draw) {
			heart.push(bloom);
			garden.createRandomBloom(bloom[0], bloom[1]);
		}
	}, interval);
}

(function($) {
	$.fn.typewriter = function() {
		this.each(function() {
			var $ele = $(this), str = $ele.html(), progress = 0;
			$ele.html('');
			var timer = setInterval(function() {
				var current = str.substr(progress, 1);
				if (current == '<') {
					progress = str.indexOf('>', progress) + 1;
				} else {
					progress++;
				}
				$ele.html(str.substring(0, progress) + (progress & 1 ? '_' : ''));
				if (progress >= str.length) {
					clearInterval(timer);
				}
			}, 100);
		});
		return this;
	};
})(jQuery);

var beginDate = new Date();
beginDate.setFullYear(2011, 0, 22);

function timeElapse(date){
	var current = Date();
	//var seconds = (Date.parse(current) - Date.parse(date)) / 1000;
	var seconds = (Date.parse(date) - Date.parse(current)) / 1000;
	if (seconds < 0) {
		seconds = (Date.parse(current) - Date.parse(beginDate)) / 1000;
	}
	var days = Math.floor(seconds / (3600 * 24));
	seconds = seconds % (3600 * 24);
	var hours = Math.floor(seconds / 3600);
	if (hours < 10) {
		hours = "0" + hours;
	}
	seconds = seconds % 3600;
	var minutes = Math.floor(seconds / 60);
	if (minutes < 10) {
		minutes = "0" + minutes;
	}
	seconds = seconds % 60;
	if (seconds < 10) {
		seconds = "0" + seconds;
	}
	var result = "<span class=\"digit\">" + days + "</span> days <span class=\"digit\">" + hours + "</span> hours <span class=\"digit\">" + minutes + "</span> minutes <span class=\"digit\">" + seconds + "</span> seconds"; 
	$("#elapseClock").html(result);
}

function showMessages() {
	adjustWordsPosition();
	$('#messages').fadeIn(5000, function() {
		showLoveU();
	});
}

function adjustWordsPosition() {
	$('#words').css("position", "absolute");
	//$('#words').css("top", $("#garden").position().top + 195);
	$('#words').css("top", $("#garden").position().top);
	$('#words').css("left", $("#garden").position().left + 70);
}

function adjustCodePosition() {
	$('#code').css("margin-top", ($("#garden").height() - $("#code").height()) / 2 - 100);
}

function showLoveU() {
	$('#loveu').fadeIn(3000);
}