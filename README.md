### Day 08 - 打造基本後台(4) - Menu

首先加入`MatMenuModule`在`src\app\shared-material\shared-material.module.ts`

#### 使用mat-menu

我們使用`<mat-menu>`來為原來toolbar的message icon加上Menu選單，首先我們可以再任意地方加入`<mat-menu>`與`<mat-menu-item>`組合的選單。

```html
<!-.......-->
</mat-sidenav-container>
<mat-menu #messageMenu="matMenu">
  <button mat-menu-item>最新訊息</button>
  <button mat-menu-item>訊息設定</button>
</mat-menu>
```

這時候畫面上還不會有任何資料，還需需要一個可以觸發顯示選單的來源，我們加在toolbar上的message icon上

```html
<button mat-icon-button [matMenuTriggerFor]="messageMenu" #menuTrigger="matMenuTrigger">
    <mat-icon>message</mat-icon>
  </button>
```

上面的程式我們透過`matMenuTriggerFor`來設定這個按鈕會觸發哪一個menu元件。點擊看看原來的message icon按鈕，可以發現menu選單就跳出來啦！這時候我們可以使用上下鍵來切換focus的選項，也可以按下Enter鍵來觸發按鈕的click事件，操作上超級方便的！

#### 在程式中開啟選單

要在程式中開啟選單很容易，只要找到`matMenuTriggerFor`，再觸發他的`openMenu()`方法就可以打開menu，透過`closeMenu()`則能夠動態的關閉menu，另外我們也可以透過`toggleMenu()`來開關menu的顯示狀態在`src\app\dashboard\dashboard.component.html`中找到之的`我是左邊選單`修改如下

```
<button mat-raised-button (click)="menuTrigger.toggleMenu()">開啟訊息設定</button>
```

#### 使用xPosition和yPosition調整選單出現位置

`<mat-menu>`預設會在出現時把整個menuTrigger覆蓋掉，同時也會根據目前所在的位置來決定選單生長的方向，例如按鈕在畫面的右邊的話，選單預設會往左邊長，在畫面上方的話，選單預設會往下長，總之就是會盡量讓整個menu在畫面中可以被看到就對了，我們可以能夠透過`xPosition`和`yPosition`，來控制生長的方向

- **xPosition**：選項為`after`(預設值、從start往end的方向長，通常是從左到右)或`before`(從end往start的方向長，通常是從右到左)。
- **yPosition**：選項為`below`(預設值、從上往下長)或`above`(從下往上長)

舉例來說，一個沒有設定`xPosition`和`yPosition`，且生長方向沒有阻礙時的程式碼

```html
<div style="text-align: center">
  <button mat-raised-button [matMenuTriggerFor]="positionMenu">開啟訊息設定，這是一條比較長的按鈕，好確認Menu的生長方向</button>
  <mat-menu #positionMenu="matMenu">
    <button mat-menu-item>訊息1</button>
    <button mat-menu-item>訊息2</button>
  </mat-menu>
</div>
```

當我們調整`xPosition`和`yPosition`如下時：

```html
<mat-menu #positionMenu="matMenu" xPosition="before" yPosition="above">
  <button mat-menu-item>訊息1</button>
  <button mat-menu-item>訊息2</button>
</mat-menu>
```

可以看到生長的方向就變了，有趣的是當我們往下捲動螢幕時由於menu即將碰到頂端，會立刻改爲往下生長，盡可能讓選單可以被看到，**只能說Angular Material既貼心又聰明啊！**

另外，我們也可以設定`[overlapTrigger]="false"`，如此一來，選單就永遠不會遮住我們的trigger：

```html
<mat-menu #messageMenu="matMenu" [overlapTrigger]="false">
  <button mat-menu-item>最新訊息</button>
  <button mat-menu-item>訊息設定</button>
</mat-menu>
<button mat-icon-button [matMenuTriggerFor]="messageMenu" #menuTrigger="matMenuTrigger">
  <mat-icon>message</mat-icon>
</button>
```

### 巢狀menu

menu選單可以是巢狀的，要使用巢狀的選單沒有什麼新技巧，一樣把子選單使用`<mat-menu>`設計好，然後在原來的選單選項中加入`matMenuTriggerFor`即可：

```html
<div style="text-align: center">
  <button mat-raised-button [matMenuTriggerFor]="nestMenu">巢狀選單demo</button>
  <mat-menu #nestMenu="matMenu">
    <button mat-menu-item [matMenuTriggerFor]="subMenu1">訊息1</button>
    <button mat-menu-item [matMenuTriggerFor]="subMenu2">訊息2</button>
    <button mat-menu-item>訊息3</button>
  </mat-menu>
  <mat-menu #subMenu1="matMenu">
    <button mat-menu-item>
      <mat-icon>persion</mat-icon>
      訊息 1-1
    </button>
    <button mat-menu-item>
      <mat-icon>favorite</mat-icon>
      訊息 1-2
    </button>
    <button mat-menu-item>
      <mat-icon>thumb_up</mat-icon>
      訊息 1-3
    </button>
  </mat-menu>

  <mat-menu #subMenu2="matMenu">
    <button mat-menu-item>
      <mat-icon>delete</mat-icon>
      訊息 2-1
    </button>
    <button mat-menu-item disabled>
      <mat-icon>settings</mat-icon>
      訊息 2-2
    </button>
  </mat-menu>

</div>
```

### 小技巧：搭配使用mat-divider

當要呈現的選單多的時候，除了選擇用巢狀的選單以外，用一個divider分隔也是個不錯的選擇，可以減少子項目難以分類的煩惱，在行動裝置的呈現上也會比較清楚。

```html
<mat-menu #positionMenu="matMenu">
  <button mat-menu-item [matMenuTriggerFor]="subMenu1">訊息1</button>
  <button mat-menu-item [matMenuTriggerFor]="subMenu2">訊息2</button>
  <mat-divider></mat-divider>
  <button mat-menu-item>訊息3</button>
</mat-menu>
```

























###### 參考網站

[Angular Material完全攻略 系列](https://ithelp.ithome.com.tw/users/20020617/ironman/1263)

