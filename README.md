# 웹 쿠키런

<img width="1178" alt="1" src="https://user-images.githubusercontent.com/107898063/212458533-d26c4952-6dc3-44be-ae00-85c73984501d.png">

<br/>

## Canvas와 JavaScript를 활용한 쿠키런 웹게임
<br/>
<br/>

### 1. 사용기술
#
- JavaScript(Canvas)

<br/>

### 2. 게임방법
#
- **코드를 다운 받으신 후 game.html 라이브서버를 켜시고 게임을 시작해주세요!**

<br/>

### 3. 게임화면
#

- **플레이 방법**

<img width="600" alt="스크린샷 2023-01-14 오후 3 20 46" src="https://user-images.githubusercontent.com/107898063/212459052-19a8373a-f595-4e9d-9ce2-60d8cc939fa8.png">
<br/>
<br/>

- **달리기**

<img width="600" alt="스크린샷 2023-01-14 오후 3 21 09" src="https://user-images.githubusercontent.com/107897886/217739986-8aefe891-72bd-4b14-b80b-80afbb881457.gif">
<br/>

```javascript
let player = {
  x: 120,
  y: 400,
  width: 80,
  height: 90,
  yspeed: 0,
  index: 0,
  speed: 3,
  time: 0,
  state: "run",
  draw() {
    this.time++;
    if (this.time % this.speed === 0) {
      if (this.index < 3) {
        this.index++;
      } else {
        this.index = 0;
      }
    }
  }
```
```javascript
ctxMain.drawImage(
      this.state == "run"
        ? runPlayer[this.index])
```
```javascirpt
let runPlayer = new Array();
let imglinkRun = [
  "images/Character/Taehoon/Run/Run1.png",
  "images/Character/Taehoon/Run/Run2.png",
  "images/Character/Taehoon/Run/Run3.png",
  "images/Character/Taehoon/Run/Run4.png",
];
```
기본적으로 플레이어의 값은 캔버스의 위치에 고정되어 있고 캐릭터의 상태에 따라서 그려주는 이미지의 배열을 변경

<br/>

- **점프와 더블 점프**

<img width="600" alt="스크린샷 2023-01-14 오후 3 21 09" src="https://user-images.githubusercontent.com/107897886/217746462-28f4eaaf-d8d6-4bf0-9815-de42ca0a9888.gif">

```javascript
function jumpSkill() {
  //점프시 점프값 증가 & 이미지 변경
  if (player.state == "jumpstart" && jumpTimer > 30) {
    player.state = "jump";
  }

  if (jump == true) {
    player.y -= 7.57;
    jumpTimer++;
  }

  //더블점프
  if (dbjump == true) {
    player.y -= 0.45;
    jumpTimer++;
  }

  if (falling == true) {
    player.y -= 7.57;
    jumpTimer++;
  }

  //더블 점프 이미지 변경
  if (player.state == "dbjumpstart" && jumpTimer > 30) {
    player.state = "dbjump";
  }
  if (player.state == "dbjump" && jumpTimer > 110) {
    player.state = "dbjumplast";
  }
  //더블 점프 & 점프타이머 100 넘어가면 상승 끝
  if (player.state == "dbjump" && jumpTimer > 10) {
    player.y -= 0;
  }
  if (player.state == "falling" && jumpTimer > 100) {
    let fallingcome = setInterval(() => {
      player.y -= 0;
    }, 1);
    setTimeout(() => {
      clearInterval(fallingcome);
    }, 3000);
  }
}
```
스페이스바를 눌러 캐릭터의 상태값이 점프로 변하면 점프시간을 증가시키고 그 점프시간에 따라 캐릭터의 상태와 이미지 배열을 바꿔주어 점프와 더블점프시 다양한 이미지가 노출되도록 구현

<br/>

- **충돌**

<img width="600" alt="스크린샷 2023-01-14 오후 3 21 09" src="https://user-images.githubusercontent.com/107897886/217748533-5619aaa5-9b77-488e-b434-b81f8248824a.gif">

```javascript
function TopHurdle() {
  //앉는 허들
  for (let i = 0; i < hurdle.length; i++) {
    col_left_X = hurdle[i].x - player.x;
    col_right_X = hurdle[i].x + hurdle[i].width - (player.x + player.width);
    if (
      col_left_X < 85 &&
      col_left_X > -100 &&
      (col_right_X < 100) & (col_right_X > -30) &&
      player.state != "slide" &&
      a == true
    ) {
      col_temp--;
      console.log(col_temp);
      console.log("충돌");
      player.state = "coll";
      a = false;
      hpRemove();
      let hi = setInterval(() => {
        a = false;
        player.state = "coll";
      }, 1);
      setTimeout(() => {
        clearInterval(hi);
        player.state = "run";
        a = true;
      }, 700);
    }
  }
}
```
충돌시 캐릭터의 상태를 충돌로 바꾸고 캐릭터의 상태가 충돌일시 HP가 일정량, 일정시간 간격으로 줄어들도록 구현 <br/>
젤리도 마찬가지로 캐릭터의 X,Y 값과 젤리의 X,Y의 값을 계산해서 일정영역 안으로 들어오면 젤리가 사라지고 점수가 증가하도록 구현 
<br/>
<br/>


### 4. 이슈
#


(1) 높은 장애물을 올라타고 점프를 하여 캐릭터의 Y좌표의 값이 -가 될 경우 끝없이 Y좌표의 값이 무한대로 마이너스되는 이슈가 발생

    테스트를 하면서 Y의 값이 움직이는 변수 yspeed의 값을 삭제시켜서  문제 발생 - yspeed의 값과 gravity의 적당한 조절로 해결
<br/>

(2) floor에 올라타게 되거나 점프하고 착지할 때, 추락하는 문제가 발생

    floor와 캐릭터가 충돌했을 때 닿는 값을 고정값으로 줬기 때문에 문제가 발생했다는 것을 확인. 충돌 값을 범위로 설정해서 해결.
    이후 장애물이나 젤리도 같은 방법으로 구현

    